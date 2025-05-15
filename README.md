// server.js
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');

const app = express();
app.use(cors());
app.use(express.json());

// Connect to MongoDB
mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost/freelance-platform', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Define Gig model
const Gig = mongoose.model('Gig', {
  title: String,
  description: String,
  price: Number,
  freelancer: String,
});

// Routes

// POST: Create a gig
app.post('/gigs', async (req, res) => {
  const gig = new Gig(req.body);
  await gig.save();
  res.send(gig);
});

// GET: Get all gigs
app.get('/gigs', async (req, res) => {
  const gigs = await Gig.find();
  res.send(gigs);
});

// Start server
const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
