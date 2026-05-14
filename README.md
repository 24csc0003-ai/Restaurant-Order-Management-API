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

