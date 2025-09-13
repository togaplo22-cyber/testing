{
  "name": "investment-backend",
  "version": "1.0.0",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "bcryptjs": "^2.4.3",
    "cors": "^2.8.5",
    "dotenv": "^16.3.1",
    "express": "^4.18.2",
    "jsonwebtoken": "^9.0.1",
    "mongoose": "^7.5.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");
require("dotenv").config();

const authRoutes = require("./routes/auth");
const transactionRoutes = require("./routes/transactions");
const investmentRoutes = require("./routes/investment");
const adminRoutes = require("./routes/admin");

const app = express();
app.use(cors());
app.use(express.json());

app.use("/api/auth", authRoutes);
app.use("/api/transactions", transactionRoutes);
app.use("/api/investment", investmentRoutes);
app.use("/api/admin", adminRoutes);

mongoose.connect(process.env.MONGO_URI)
  .then(() => console.log("MongoDB connected"))
  .catch(err => console.error(err));

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on ${PORT}`));
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  phone: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  referralCode: { type: String, unique: true },
  referredBy: { type: String, default: null },
  walletBalance: { type: Number, default: 0 },
  team: {
    level1: [{ type: mongoose.Schema.Types.ObjectId, ref: "User" }],
    level2: [{ type: mongoose.Schema.Types.ObjectId, ref: "User" }],
    level3: [{ type: mongoose.Schema.Types.ObjectId, ref: "User" }]
  },
  investments: [
    { amount: Number, dailyIncome: Number, date: { type: Date, default: Date.now } }
  ],
  transactions: [
    { type: { type: String }, amount: Number, fee: Number, status: String, date: { type: Date, default: Date.now } }
  ],
  isAdmin: { type: Boolean, default: false }
});

module.exports = mongoose.model("User", userSchema);
const mongoose = require("mongoose");

const transactionSchema = new mongoose.Schema({
  userId: { type: mongoose.Schema.Types.ObjectId, ref: "User" },
  type: { type: String, enum: ["deposit", "withdraw"] },
  amount: Number,
  fee: Number,
  status: { type: String, enum: ["pending","completed","failed"], default: "pending" },
  date: { type: Date, default: Date.now }
});

module.exports = mongoose.model("Transaction", transactionSchema);
const mongoose = require("mongoose");

const giftCodeSchema = new mongoose.Schema({
  code: { type: String, required: true, unique: true },
  amount: { type: Number, required: true },
  usedBy: { type: mongoose.Schema.Types.ObjectId, ref: "User", default: null }
});

module.exports = mongoose.model("GiftCode", giftCodeSchema);
