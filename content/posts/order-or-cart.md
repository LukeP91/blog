---
title: Order or cart
date: 2016-09-03T20:21:43+01:00
---

Today I am going to write about another interesting part of my shopping cart project. On of interesting part was deciding how should I handle creating order.

Current user can add products to shopping cart and then create order. Making order doesn't have any functionality except creating a new order and removing all products from a cart.

When I was trying to figure out the best possible way to tackle that problem I come up with three solutions. I will try to give you some pros and cons for each of them. At least how I see them.

### "Easy way"
The first way appeared to be the easiest solution that required minimal work on my side. In it, I assumed that since in my project order and cart don't have any functional differences I could reuse Cart model and simply add a field that would mark it as ordered. It looked simple, fast what could go wrong, right?

Well, first of all, the assumption that nothing will change and functionality will stay the same for the whole project is let's say brave. Even slight changes to order or cart would require serious effort to make it work. Almost each function would need to check if it was called for a cart or an order. Any changes to how they behave would cause serious headaches.

And the second problem as important. Order and Cart are completely two different things in reality so why should we treat them as the same thing in code. Cart is something that is "fluid" it can change any minute and order generally is constant. When made it doesn't change barring any special cases.

### "Seemed ok way"
Next solution that I initially thought was the best to applied here was to make order and cart separate models but make both of them share cart_item model which would be renamed to just item order. It looked like it solves all the issues with first way. We separate order and cart, which now can have different functionalities and we don't create an extra model to store the same information as between order and cart. Looks good for me. But it is not a good one and I will try to explain myself when describing the last solution.

### "The right way"
You can probably figure out the right way now by yourself. It turns out the best way was to create separate models for order, cart, cart\_item and order_item. Why separte order and cart I have explained above. Now we just need to figure out why order item is different than cart item.

First, they are completely two different contexts in our application and in real life and should be properly separated.

Second thing order item should store price, taxes for the product that were true at the time of making the order not now. In cart item, we store just reference to product and not actual values. So when anything about it changes like its price or tax it is reflected in the cart. But such thing should not happen in order. When we buy something we want to get an invoice for a price that was during making our order, not the price that is now. Also, any changes in taxes don't affect orders that were made before that change.

Because of that and the fact that making one more model doesn't add much complexity or requires a lot of extra time, I believe third solution is the best one.
