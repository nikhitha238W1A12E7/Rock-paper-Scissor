# Rock-paper-Scissor
Rock-Paper-Scissor using Mern Stak Development
I created a Rock Paper Scissors game using the MERN stack, which includes MongoDB, Express, React, and Node.js. The game allows two players to play against each other through a clean and interactive interface made with React.

On the backend, I used Node.js and Express to handle the game logic and store the results in MongoDB. This way, the game can keep track of scores and game history.

Using React, I made the game responsive and updated the screen based on the players’ choices in real-time. This project helped me learn how to build a full web application with both front-end and back-end working together smoothly.

#code for rock-paper-scissor
games.js
const express = require('express');
const router = express.Router();
const Game = require('../models/Game');

const choices = ['rock', 'paper', 'scissors'];

function getResult(player, computer) {
  if (player === computer) return 'draw';
  if (
    (player === 'rock' && computer === 'scissors') ||
    (player === 'paper' && computer === 'rock') ||
    (player === 'scissors' && computer === 'paper')
  ) return 'win';
  return 'lose';
}

// GET all games
router.get('/', async (req, res) => {
  try {
    const games = await Game.find().sort({ createdAt: -1 });
    res.json(games);
  } catch (err) {
    res.status(500).json({ error: 'Failed to fetch games' });
  }
});

// POST a new game
router.post('/', async (req, res) => {
  const { playerChoice } = req.body;
  if (!choices.includes(playerChoice)) {
    return res.status(400).json({ error: "Invalid choice" });
  }
  const computerChoice = choices[Math.floor(Math.random() * 3)];
  const result = getResult(playerChoice, computerChoice);

  const game = new Game({ playerChoice, computerChoice, result });
  await game.save();

  res.json({ playerChoice, computerChoice, result });
});

module.exports = router;
