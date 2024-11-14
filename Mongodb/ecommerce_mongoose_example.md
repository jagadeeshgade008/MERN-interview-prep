
# E-commerce Example with Mongoose in Node.js

This example demonstrates an e-commerce database structure using **Node.js** with **Mongoose**. The data model includes **Users**, **Orders**, **Products**, and **Reviews**, showcasing complex relationships.

## 1. Models

### User Model (`User.js`)

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const userSchema = new Schema({
  name: { type: String, required: true },
  email: { type: String, unique: true, required: true },
  orders: [{ type: Schema.Types.ObjectId, ref: 'Order' }],
  reviews: [{ type: Schema.Types.ObjectId, ref: 'Review' }]
}, { timestamps: true });

module.exports = mongoose.model('User', userSchema);
```

### Product Model (`Product.js`)

```javascript
const productSchema = new Schema({
  name: { type: String, required: true },
  price: { type: Number, required: true },
  category: { type: String, required: true },
  reviews: [{ type: Schema.Types.ObjectId, ref: 'Review' }]
}, { timestamps: true });

module.exports = mongoose.model('Product', productSchema);
```

### Order Model (`Order.js`)

```javascript
const orderSchema = new Schema({
  user: { type: Schema.Types.ObjectId, ref: 'User', required: true },
  products: [{ type: Schema.Types.ObjectId, ref: 'Product', required: true }],
  total: { type: Number, required: true },
  status: { type: String, enum: ['pending', 'completed', 'cancelled'], default: 'pending' }
}, { timestamps: true });

module.exports = mongoose.model('Order', orderSchema);
```

### Review Model (`Review.js`)

```javascript
const reviewSchema = new Schema({
  user: { type: Schema.Types.ObjectId, ref: 'User', required: true },
  product: { type: Schema.Types.ObjectId, ref: 'Product', required: true },
  rating: { type: Number, min: 1, max: 5, required: true },
  comment: { type: String, required: true }
}, { timestamps: true });

module.exports = mongoose.model('Review', reviewSchema);
```

## 2. Sample Queries

### Creating and Saving Documents with Relationships

#### Create a User

```javascript
const User = require('./models/User');

async function createUser(name, email) {
  const user = new User({ name, email });
  return await user.save();
}

// Usage
createUser('John Doe', 'john@example.com').then(console.log);
```

#### Create a Product

```javascript
const Product = require('./models/Product');

async function createProduct(name, price, category) {
  const product = new Product({ name, price, category });
  return await product.save();
}

// Usage
createProduct('Smartphone', 799, 'Electronics').then(console.log);
```

#### Create an Order for a User with Products

```javascript
const Order = require('./models/Order');

async function createOrder(userId, productIds) {
  const total = await Product.find({ _id: { $in: productIds } })
    .then(products => products.reduce((sum, p) => sum + p.price, 0));
  
  const order = new Order({ user: userId, products: productIds, total });
  return await order.save();
}

// Usage
createOrder('USER_ID_HERE', ['PRODUCT_ID_1', 'PRODUCT_ID_2']).then(console.log);
```

#### Create a Review for a Product by a User

```javascript
const Review = require('./models/Review');

async function createReview(userId, productId, rating, comment) {
  const review = new Review({ user: userId, product: productId, rating, comment });
  return await review.save();
}

// Usage
createReview('USER_ID_HERE', 'PRODUCT_ID_HERE', 5, 'Great product!').then(console.log);
```

### Fetching Data with Populated References

#### Get a User with Orders and Reviews

```javascript
async function getUserWithDetails(userId) {
  return await User.findById(userId)
    .populate({
      path: 'orders',
      populate: { path: 'products' }
    })
    .populate('reviews')
    .exec();
}

// Usage
getUserWithDetails('USER_ID_HERE').then(console.log);
```

#### Get a Product with Reviews and Reviewers’ Details

```javascript
async function getProductWithReviews(productId) {
  return await Product.findById(productId)
    .populate({
      path: 'reviews',
      populate: { path: 'user' }
    })
    .exec();
}

// Usage
getProductWithReviews('PRODUCT_ID_HERE').then(console.log);
```

#### Get an Order with User and Product Details

```javascript
async function getOrderWithDetails(orderId) {
  return await Order.findById(orderId)
    .populate('user')
    .populate('products')
    .exec();
}

// Usage
getOrderWithDetails('ORDER_ID_HERE').then(console.log);
```

## 3. Complex Aggregation Examples

### a. Calculate Average Product Rating

Get the average rating for a specific product based on its reviews.

```javascript
const Product = require('./models/Product');
const Review = require('./models/Review');

async function getAverageRating(productId) {
  const result = await Review.aggregate([
    { $match: { product: productId } },
    { $group: { _id: "$product", averageRating: { $avg: "$rating" } } }
  ]);
  return result.length > 0 ? result[0].averageRating : null;
}

// Usage
getAverageRating('PRODUCT_ID_HERE').then(console.log);
```

### b. Get Top-rated Products

Retrieve products with an average rating above a certain threshold.

```javascript
async function getTopRatedProducts(minRating) {
  const topProducts = await Review.aggregate([
    { $group: { _id: "$product", avgRating: { $avg: "$rating" } } },
    { $match: { avgRating: { $gte: minRating } } },
    { $lookup: {
      from: "products",
      localField: "_id",
      foreignField: "_id",
      as: "productDetails"
    }},
    { $unwind: "$productDetails" }
  ]);
  return topProducts;
}

// Usage
getTopRatedProducts(4).then(console.log);
```

### c. Get Total Order Value by User

Calculate the total amount spent by each user on all orders.

```javascript
async function getTotalSpentByUser(userId) {
  const totalSpent = await Order.aggregate([
    { $match: { user: userId } },
    { $group: { _id: "$user", totalSpent: { $sum: "$total" } } }
  ]);
  return totalSpent.length > 0 ? totalSpent[0].totalSpent : 0;
}

// Usage
getTotalSpentByUser('USER_ID_HERE').then(console.log);
```

### d. Get Monthly Sales Report

Generate a report of total sales for each month.

```javascript
async function getMonthlySalesReport() {
  const salesReport = await Order.aggregate([
    { $match: { status: "completed" } },
    { $group: {
      _id: { $month: "$createdAt" },
      totalSales: { $sum: "$total" },
      ordersCount: { $sum: 1 }
    }},
    { $sort: { "_id": 1 } }
  ]);
  return salesReport;
}

// Usage
getMonthlySalesReport().then(console.log);
```

## 4. Documentation Summary

- **User**: Contains user details and references to `Order` and `Review`.
- **Product**: Contains product details and references to `Review`.
- **Order**: Connects a `User` with `Products` and tracks order status.
- **Review**: Links a `User` to a `Product` with a rating and comment.

This setup provides a robust way to handle complex relationships in MongoDB with Mongoose in Node.js, supporting queries that populate related data and complex aggregations.
