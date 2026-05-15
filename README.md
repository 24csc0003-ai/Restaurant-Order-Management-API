# Restaurant-Order-Management-API
Restaurants need a digital system to manage their menu and customer orders. Currently, they use paper or spreadsheets which are error-prone and slow.
restaurant-api
│
├── models
│   ├── MenuItem.js
│   └── Order.js
│
├── controllers
│   ├── menuController.js
│   └── orderController.js
│
├── routes
│   ├── menuRoutes.js
│   └── orderRoutes.js
│
├── .env
├── .gitignore
├── package.json
├── README.md
└── server.js

npm init -y
npm install express mongoose dotenv nodemon

"scripts": {
  "start": "node server.js",
  "dev": "nodemon server.js"
}

PORT=5000
MONGO_URI=your_mongodb_connection_string

const express = require("express");
const mongoose = require("mongoose");
const dotenv = require("dotenv");

dotenv.config();

const app = express();

app.use(express.json());

const menuRoutes = require("./routes/menuRoutes");
const orderRoutes = require("./routes/orderRoutes");

app.use("/api/menu", menuRoutes);
app.use("/api/orders", orderRoutes);

mongoose.connect(process.env.MONGO_URI)
.then(() => {
    console.log("MongoDB Connected");

    app.listen(process.env.PORT, () => {
        console.log(`Server running on port ${process.env.PORT}`);
    });
})
.catch((err) => console.log(err));

const mongoose = require("mongoose");

const MenuItemSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true,
        minlength: 3
    },

    price: {
        type: Number,
        required: true,
        min: 1
    },

    category: {
        type: String,
        enum: ["Pizza", "Burger", "Salad", "Beverage", "Dessert"]
    },

    ingredients: [String],

    available: {
        type: Boolean,
        default: true
    }

}, { timestamps: true });

module.exports = mongoose.model("MenuItem", MenuItemSchema);

const mongoose = require("mongoose");

const OrderSchema = new mongoose.Schema({

    customerName: {
        type: String,
        required: true,
        minlength: 2
    },

    customerPhone: String,

    items: [
        {
            name: String,
            quantity: {
                type: Number,
                min: 1
            },
            price: Number
        }
    ],

    totalAmount: Number,

    status: {
        type: String,
        enum: ["pending", "preparing", "ready", "completed", "cancelled"],
        default: "pending"
    }

}, { timestamps: true });

module.exports = mongoose.model("Order", OrderSchema);

const MenuItem = require("../models/MenuItem");

exports.createMenuItem = async (req, res) => {
    try {
        const item = await MenuItem.create(req.body);
        res.status(201).json(item);
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};

exports.getMenuItems = async (req, res) => {
    const items = await MenuItem.find();
    res.json(items);
};

exports.getSingleMenuItem = async (req, res) => {
    try {
        const item = await MenuItem.findById(req.params.id);

        if (!item) {
            return res.status(404).json({ message: "Item not found" });
        }

        res.json(item);

    } catch (error) {
        res.status(500).json({ message: error.message });
    }
};

exports.updateMenuItem = async (req, res) => {
    const item = await MenuItem.findByIdAndUpdate(
        req.params.id,
        req.body,
        { new: true }
    );

    res.json(item);
};

exports.deleteMenuItem = async (req, res) => {
    await MenuItem.findByIdAndDelete(req.params.id);

    res.json({ message: "Menu item deleted" });
};

const Order = require("../models/Order");

exports.createOrder = async (req, res) => {
    try {
        const order = await Order.create(req.body);
        res.status(201).json(order);
    } catch (error) {
        res.status(400).json({ message: error.message });
    }
};

exports.getOrders = async (req, res) => {
    const orders = await Order.find();
    res.json(orders);
};

exports.getSingleOrder = async (req, res) => {
    const order = await Order.findById(req.params.id);

    if (!order) {
        return res.status(404).json({ message: "Order not found" });
    }

    res.json(order);
};

exports.updateOrderStatus = async (req, res) => {

    const order = await Order.findByIdAndUpdate(
        req.params.id,
        { status: req.body.status },
        { new: true }
    );

    res.json(order);
};

exports.deleteOrder = async (req, res) => {
    await Order.findByIdAndDelete(req.params.id);

    res.json({ message: "Order deleted" });
};

const express = require("express");
const router = express.Router();

const {
    createMenuItem,
    getMenuItems,
    getSingleMenuItem,
    updateMenuItem,
    deleteMenuItem
} = require("../controllers/menuController");

router.post("/", createMenuItem);
router.get("/", getMenuItems);
router.get("/:id", getSingleMenuItem);
router.patch("/:id", updateMenuItem);
router.delete("/:id", deleteMenuItem);

module.exports = router;

node_modules
.env

# Restaurant API

Simple Restaurant Order Management API built with:

- Node.js
- Express.js
- MongoDB

## Features

- Create menu items
- Get menu items
- Update menu items
- Delete menu items
- Place orders
- Update order status

## Run Project

Install dependencies:

npm install

Run server:

npm run dev

Server URL:

http://localhost:5000

npm run dev

POST http://localhost:5000/api/menu

{
  "name": "Burger",
  "price": 2000,
  "category": "Burger",
  "ingredients": ["Bread", "Meat"]
}

git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin YOUR_REPOSITORY_LINK
git push -u origin main








