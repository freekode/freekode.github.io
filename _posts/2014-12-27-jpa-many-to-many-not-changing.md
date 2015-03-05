---
layout: post
title: JPAContainer, many-to-many not changing
---

Once upon a time... I haved a problem with setting entities which are connected with many-to-many relationship.

I in both enitites I just declared one field is type of Set<>, with standart code:
 
{% highlight java %}
@ManyToMany
@JoinTable(name = "orders_items",
        joinColumns = {@JoinColumn(name = "orderId")},
        inverseJoinColumns = {@JoinColumn(name = "itemId")})
private Set<Item> items = new HashSet<>();
{% endhighlight %}

But when I started setting this relation. Nothing was in the database.

Ok, lets go to the solution. With many-to-many relationship, you should change not field with @ManyToMany annotation. You should use association entity (OrderItem).

I have three entities, with many-to-many relationship (Order, Item, OrderItem).
{% highlight java %}
@Entity
@Table(name = "orders", uniqueConstraints = {@UniqueConstraint(columnNames = {"id"})})
public class Order {
    /* other fields...*/

    @ManyToMany
    @JoinTable(name = "orders_items",
            joinColumns = {@JoinColumn(name = "orderId")},
            inverseJoinColumns = {@JoinColumn(name = "itemId")})
    private Set<Item> items = new HashSet<>();

    @OneToMany(mappedBy = "order")
    private Set<OrderItem> orderItems = new HashSet<>();

    /* constructors, getters, setters... */
}


@Entity
@Table(name = "orders", uniqueConstraints = {@UniqueConstraint(columnNames = {"id"})})
public class Order {
    /* other fields...*/

    @ManyToMany
    @JoinTable(name = "orders_items",
            joinColumns = {@JoinColumn(name = "orderId")},
            inverseJoinColumns = {@JoinColumn(name = "itemId")})
    private Set<Item> items = new HashSet<>();

    @OneToMany(mappedBy = "order")
    private Set<OrderItem> orderItems = new HashSet<>();

    /* constructors, getters, setters... */
}


@Entity
@Table(name = "orders_items", uniqueConstraints = {@UniqueConstraint(columnNames = {"id"})})
public class OrderItem {
    /* other fields...*/

    @ManyToOne
    @JoinColumn(name = "orderId")
    private Order order;

    @ManyToOne
    @JoinColumn(name = "itemId")
    private Item item;

    /* constructors, getters, setters... */
}
{% endhighlight %}

After that you can set relationship like that:
{% highlight java %}
JPAContainer<Order> orderJpa = JPAContainerFactory.make(Order.class, "persistence");
Order order = orderJpa.getItem(1).getEntity();

Set<OrderItem> orderItems = ....
order.setOrderItems(orderItems);

orderJpa.commit();
{% endhighlight %}

And everything should be work properly.
