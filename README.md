npm init -y

npm install express

const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json()); // Middleware to parse JSON bodies

app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});


let books = [
  { id: 1, title: "1984", author: "George Orwell" },
  { id: 2, title: "The Hobbit", author: "J.R.R. Tolkien" }
];


app.get('/books', (req, res) => {
  res.json(books);
});


app.post('/books', (req, res) => {
  const { id, title, author } = req.body;
  books.push({ id, title, author });
  res.status(201).json({ message: 'Book added', books });
});


app.put('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const { title, author } = req.body;

  const book = books.find(b => b.id === bookId);
  if (book) {
    book.title = title || book.title;
    book.author = author || book.author;
    res.json({ message: 'Book updated', book });
  } else {
    res.status(404).json({ message: 'Book not found' });
  }
});


app.delete('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const index = books.findIndex(b => b.id === bookId);

  if (index !== -1) {
    books.splice(index, 1);
    res.json({ message: 'Book deleted', books });
  } else {
    res.status(404).json({ message: 'Book not found' });
  }
});

