# Web Server Codealong

### Learning Objectives

Students should be able to:

- Use Postman to send requests, including ones with a body.
- Use `req.body` to get extra information from the client.

### Introduction

This codealong is for a note-taking API that will use CRUD operations to manipulate the notes in a JSON file. The `data.json` file already exists—you'll find it in the `data` directory.

Your job will be to _extend_ the JavaScript file that's _already_ in the `src` directory—it has several request handlers already, but it needs to handle a couple more requests that are a bit more complex.

### Resources

#### Postman

The Postman documentation online is _excellent_. Here are a few short articles to get you started.

- [Your first steps with Postman](https://learning.postman.com/docs/getting-started/first-steps/overview/). Reading "Overview", "Download", and "Send a request" will get you pretty far!
- [Send requests](https://learning.postman.com/docs/sending-requests/requests/) is another very relevant section, diving further into how to send requests with Postman. Keep in mind that, for this lab, we a) always want to send a GET request, and b) will be including JSON objects in the Body of our request as "raw data".

### Repo Usage

This codealong should likely be coded by the instructor with help from the group.

Right now, this repo has:

- Example code solutions to work towards in the readme (this document).
- Two starting points in the `src` directory: one using callbacks, one using `async`/`await`. You can choose which to move forward with.
- The data to work with in `data/data.json`.
- A backup of that data in `data/backup-data.json`, in case you accidentally overwrite the data. If you do, be _sure_ to copy it over to `data.json` rather than work with `backup-data.json` directly, so that you retain a backup for when you inevitably overwrite it a second time.

### About This Codealong

#### Goal

The solution should adhere to all the specifications from the previous codealong (which should already be complete), as well as the new following API features.

##### Create A Note

Create a new note in the data file from the JSON body of the request.

###### Example Request

```
GET /create-note
Body: {"title": "Express", "text": "Express is a framework for creating web servers using Node."}
```

###### Expected Response

Arbitrary. But it should respond with _something_, or the server may hang!

This solution uses, "Note successfully written to the file!"

The important test is to check that the note has been added to the `data/data.json` file!

##### Updating A Note

Update the note at index `id` with the JSON body of the request. `id` is a pure index in the array—there is **no `id` field** in the note objects.

###### Example Request

```
GET /update-note/1
Body:   {"title": "fs.readFile", "text": "`fs.readFile` is a Node FS method for reading a local file. It takes 3 arguments: the path to the file to open (a string), the character encoding (a string, usually \"utf8\"), and a callback function. The callback receives 2 arguments: an error object if there's an error, and the data (a string), which is the contents of the file.\n\nIt can also be used with promises (and therefore `async`/`await`) if imported with `require('fs').promises`."},
```


###### Example Response

```json
{
  "title": "fs.readFile",
  "text": "`fs.readFile` is a Node FS method for reading a local file. It takes 3 arguments: the path to the file to open (a string), the character encoding (a string, usually \"utf8\"), and a callback function. The callback receives 2 arguments: an error object if there's an error, and the data (a string), which is the contents of the file.\n\nIt can also be used with promises (and therefore `async`/`await`) if imported with `require('fs').promises`."
}
```

Our solution responds with the updated note, but you should double-check that the note has been updated in the `data/data.json` file.

#### Hitting Those Endpoints

To hit these endpoints, you will need Postman or a similar tool. See the resources above. Postman is our recommendation, since it's the most popular one out there.

#### Data Location

Ideally, you should separate the data into its own directory and the code into another. This is best practice in the industry, and it's good to model that, but more immediately relevant is that if you are watching the code for changes, you do _not_ want to be watching the data file for changes as well, as this will lead to a server restart whenever the code changes the data.

Node can now run files in watch mode without an external library, as long as you have Node version 18.11 or greater, which everyone should. Simply run `node --watch [file name]` to do this, **making sure you're in the directory with the file you want to run** before you do so.

If you do not have that version of Node, or want a more established way to do this (Node having this feature is relatively new), a good way to run the server in watch mode is to install `nodemon` with `npm install --global nodemon` and run `nodemon [file name]` instead of `node [file name]`. Alternately, you can run `npx nodemon [file name]` to temporarily install `nodemon` for the length of the command, uninstalling it immediately after.

Watch mode is a great way to rapidly develop, reloading the server on every file change. You may want to turn off auto-save in your editor (it's right in the File menu in VS Code), since then you only reload the server when you explicitly want to, and not in the middle of writing a line of code (which can error out).

#### Suggestions For Running The Codealong

- You should likely walk through the `npm` commands, for this and (most likely) the next few codealongs, and be ready to help in labs with `npm` workflow mistakes.
- Decide beforehand whether you'll do `async`/`await` or callbacks, depending on where your students are at.
- Show the endpoints at work as you finish them. You can access all endpoints in this codealong and see their response using Postman.
  - When sending a Body, we recommend sending it as raw data in JSON. [The Postman documentation section on "Raw data"](https://learning.postman.com/docs/sending-requests/requests/#raw-data) has a good screenshot of how you want this to look in your Postman interface:
- You _cannot_ parse JSON without configuration of some kind. We recommend the `app.use(express.json());` line from the solutions.
- Note that these are all GET requests! We designed it this way to simplify things for their first web servers. We will expand on a better way to handle different kinds of requests when we get to REST APIs.
- You may want to review what each `app` method call, again avoiding lengthy discussions of subjects like ports and `app.use` middleware.
- Note that this is not a REST API—this was by design, as it will help us learn REST by contrasting that codealong/lab with this one.
- You could start out coding the solution below without refactoring into functions, or indeed stick with all the code in the handler and never refactor. It depends on what you want to emphasize, but overall I think time spent on how you'd refactor should not take away at this point from the main concepts. Advanced code patterns and engineering towards future change is for those who've been programming for a while.
- You might introduce `req.body` with an arbitrary object first sent from Postman. You could send an object like so:

```
End point: GET /greet-person
Body: {"name": "Colin", "age": 43}
```

Then you can write a listener in this vein:

``` javascript
app.get('/greet-person', (req, res) => {
  const name = req.body.name;
  const age = req.body.age;
  res.send("Hello, " + name + "! You are " + age + " years old.");
})
```

You could _also_ combine this with a URL parameters example, if you wish, to review the concept and contrast it with the Body.

``` javascript
app.get('/greet-person/:name', (req, res) => {
  const name = req.params.name;
  const age = req.body.age;
  res.send("Hello, " + name + "! You are " + age + " years old.");
})
```

### Solutions

Here are some immediate solutions for quick reference.

#### Cleanest Solution

This solution uses `async`/`await`. If the students don't know `async`/`await`, there is a callback-using version below.

Although this is an unrefactored version of the solution, it or the unrefactored callback version may ultimately be better solutions to demo for students than the refactored one.

```javascript
const express = require("express");
const fs = require("fs").promises;

const app = express();

app.use(express.json());

app.listen(3000, () => {
  console.log("Our notes app is now listening on http://localhost:3000");
});

app.get("/read-notes", async (req, res) => {
  const notes = await fs.readFile("../data/data.json", "utf8");

  res.send(notes);
});

app.get("/read-note/:id", async (req, res) => {
  const data = await fs.readFile("../data/data.json", "utf8");
  const notes = JSON.parse(data);
  const id = Number(req.params.id);
  for (let i = 0; i < notes.length; i++) {
    if (i === id) {
      const note = notes[i];
      const jsonVersion = JSON.stringify(note);
      res.send(jsonVersion);
    }
  }
});

app.get("/delete-note/:id", async (req, res) => {
  const id = Number(req.params.id);
  const data = await fs.readFile("../data/data.json", "utf8");
  const notes = JSON.parse(data);
  // Deletes 1 item starting at index `id`.
  // Research the splice array method for how this works!
  notes.splice(id, 1);
  const jsonVersion = JSON.stringify(notes, null, 2);
  await fs.writeFile("../data/data.json", jsonVersion, "utf8");
  res.send("Successfully deleted note.");
});

app.get("/create-note", async (req, res) => {
  const title = req.body.title;
  const text = req.body.text;
  const newNote = { title: title, text: text };
  const data = await fs.readFile("../data/data.json", "utf8");
  const notes = JSON.parse(data);
  notes.push(newNote);
  const jsonVersion = JSON.stringify(notes, null, 2);
  await fs.writeFile("../data/data.json", jsonVersion, "utf8");
  res.send("Note successfully written to the file!");
});

app.get("/update-note/:id", async (req, res) => {
  const id = Number(req.params.id);
  const updatedNote = {
    title: req.body.title,
    text: req.body.text,
  };

  const data = await fs.readFile("../data/data.json", "utf8");
  const notes = JSON.parse(data);
  for (let i = 0; i < notes.length; i++) {
    if (i === id) {
      // Deletes 1 item starting at index `id`, replacing it with the new note.
      // Research the splice array method for how this works!
      notes.splice(id, 1, updatedNote);
    }
  }

  const jsonVersion = JSON.stringify(notes, null, 2);
  await fs.writeFile("../data/data.json", jsonVersion, "utf8");
  res.send(updatedNote);
});
```

#### Callback Version

This version uses callbacks instead of `async`/`await`.

```javascript
const express = require("express");
const fs = require("fs");

const app = express();

app.use(express.json());

app.listen(3000, () => {
  console.log("Our note-taking app is now listening on http://localhost:3000");
});

app.get("/", (req, res) => {
  res.send("Hello, user!");
});

app.get("/read-notes", (req, res) => {
  fs.readFile("../data/data.json", "utf8", (err, data) => {
    res.send(data);
  });
});

app.get("/read-note/:id", (req, res) => {
  const id = Number(req.params.id);
  fs.readFile("../data/data.json", "utf8", (err, data) => {
    const notes = JSON.parse(data);
    for (let i = 0; i < notes.length; i++) {
      if (i === id) {
        const note = notes[i];
        const jsonVersion = JSON.stringify(note);
        res.send(jsonVersion);
      }
    }
  });
});

app.get("/delete-note/:id", (req, res) => {
  fs.readFile("../data/data.json", "utf8", (err, data) => {
    const id = Number(req.params.id);
    const notes = JSON.parse(data);
    // Deletes 1 item starting at index `id`.
    // Research the splice array method for how this works!
    notes.splice(id, 1);
    const jsonVersion = JSON.stringify(notes, null, 2);
    fs.writeFile("../data/data.json", jsonVersion, "utf8", (err) => {
      res.send("Successfully deleted note.");
    });
  });
});

app.get("/create-note", (req, res) => {
  const title = req.body.title;
  const text = req.body.text;
  const newNote = { title: title, text: text };
  fs.readFile("../data/data.json", "utf8", (err, data) => {
    const notes = JSON.parse(data);
    notes.push(newNote);
    const jsonVersion = JSON.stringify(notes, null, 2);
    fs.writeFile("../data/data.json", jsonVersion, "utf8", (err) => {
      res.send("Note successfully written to the file!");
    });
  });
});

app.get("/update-note/:id", (req, res) => {
  const id = Number(req.params.id);
  const title = req.body.title;
  const text = req.body.text;
  const newNote = { title: title, text: text };
  fs.readFile("../data/data.json", "utf8", (err, data) => {
    const notes = JSON.parse(data);
    for (let i = 0; i < notes.length; i++) {
      if (i === id) {
        // Deletes 1 item starting at index `id`, replacing it with the new note.
        // Research the splice array method for how this works!
        notes.splice(id, 1, newNote);
      }
    }

    const jsonVersion = JSON.stringify(notes, null, 2);
    fs.writeFile("../data/data.json", jsonVersion, "utf8", (err) => {
      res.send(newNote);
    });
  });
});
```

#### Refactored Solution

This solution uses functions to separate the request handling code from the actual CRUD actions. It also uses `async`/`await` and other advanced syntax.

```javascript
const express = require("express");
const fs = require("fs").promises;

const app = express();

app.use(express.json());

app.listen(3000, () => {
  console.log("Our note-taking app is now listening on http://localhost:3000");
});

const getAllNotes = async () => {
  return JSON.parse(await fs.readFile("../data/data.json", "utf8"));
};

const getNote = async (id) => {
  const data = await fs.readFile("../data/data.json", "utf8");
  const notes = JSON.parse(data);
  return notes.find((note, i) => i === id);
};

const deleteNote = async (id) => {
  const data = await fs.readFile("../data/data.json", "utf8");
  const notes = JSON.parse(data).filter((note, i) => i !== id);
  const jsonVersion = JSON.stringify(notes, null, 2);
  await fs.writeFile("../data/data.json", jsonVersion, "utf8");
};

const saveNote = async (newNote) => {
  const data = await fs.readFile("../data/data.json", "utf8");
  const notes = [...JSON.parse(data), newNote];
  const jsonVersion = JSON.stringify(notes, null, 2);
  await fs.writeFile("../data/data.json", jsonVersion, "utf8");
};

const updateNote = async (id, updatedNote) => {
  const data = await fs.readFile("../data/data.json", "utf8");
  const notes = JSON.parse(data).map((note, i) => {
    return i === id ? updatedNote : note;
  });

  const jsonVersion = JSON.stringify(notes, null, 2);
  await fs.writeFile("../data/data.json", jsonVersion, "utf8");
};

app.get("/read-notes", async (req, res) => {
  const notes = await getAllNotes();
  res.send(JSON.stringify(notes, null, 2));
});

app.get("/read-note/:id", async (req, res) => {
  const note = await getNote(Number(req.params.id));
  res.send(JSON.stringify(note));
});

app.get("/delete-note/:id", async (req, res) => {
  await deleteNote(Number(req.params.id));
  res.send("Successfully deleted note.");
});

app.get("/create-note", async (req, res) => {
  await saveNote({title: req.body.title, text: req.body.text});
  res.send("Note successfully written to the file!");
});

app.get("/update-note/:id", async (req, res) => {
  const updatedNote = {
    title: req.body.title,
    text: req.body.text,
  };

  await updateNote(Number(req.params.id), updatedNote);
  res.send(updatedNote);
});
```
