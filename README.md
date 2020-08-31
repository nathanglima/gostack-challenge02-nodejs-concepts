<img alt="GoStack" src="https://camo.githubusercontent.com/a869a2aaab296ef925343d7e76518cd213eb0a30/68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f676f6c64656e2d77696e642f626f6f7463616d702d676f737461636b2f6865616465722d6465736166696f732d6e65772e706e67" />

<h2 align="center">
  Challenge 01: NodeJS concepts
</h2>


## :page_facing_up: About this challenge

The objective of this challenge is to apply the knowledge acquired in NodeJS, in this first level we saw how to create a web server with Express, define the application port and manipulate some methods of HTTP protocol.

We used the HTTP protocol methods (GET, POST, PUT and DELETE) and learned some codes from the HTTP status list.

In this challenge, the functionality of this application is to register, list and update of repositories, it's also possible that we send a POST to increment likes in the repositories.

NOTE: At first we do not use a database to store the repositories, so the tests will be stored locally in an Array.

## :rocket: About of tests
Some rules for evaluating this challenge were proposed, some tests were written using jest and to pass the tests it is necessary to respect all the points below:

#### *should be able to create a new repository*

In order for this test to pass, your application must allow a repository to be created, and return a JSON with the created project.

```js
app.post("/repositories", (request, response) => {

  const { title, url, techs } = request.body;

  const repository = { id: uuid(), title, url, techs, likes: 0 };

  repositories.push(repository);

  return response.json(repository);

});
```

#### *should be able to list the repositories*

In order for this test to pass, your application must return an array with all the repositories that have been created so far.

```js
app.get("/repositories", (request, response) => {
  return response.json(repositories);  
});
```

#### *should be able to update repository*

In order for this test to pass, your application must allow only the url, title and techs fields to be changed.

```js
app.put("/repositories/:id",  (request, response) => {
 
  const { id } = request.params;
  const { title, url, techs } = request.body;

  const repositoryIndex = repositories.findIndex(repository => repository.id == id);

  if(repositoryIndex < 0){
    return response.status(400).json({ error : "Repository not found!" });
  }

  const repository = {
    id,
    title,
    url,
    techs,
    likes: repositories[repositoryIndex].likes,
  }

  repositories[repositoryIndex] = repository;

  return response.json(repository);

});
```

#### *should not be able to update a repository that does not exist*

In order for this test to pass, you must validate in your update route whether the repository id sent by the URL exists or not. If not, return an error with status 400.

```js
if(repositoryIndex < 0){
  return response.status(400).json({ error : "Repository not found!" });
}
```

#### *should not be able to update repository likes manually*

In order for this test to pass, you must not allow your update route to directly change the likes of that repository, maintaining the same number of likes that the repository already had before the update. That's because the only place that should update this information is the route responsible for increasing the number of likes.

```js
const repository = {
  id,
  title,
  url,
  techs,
  likes: repositories[repositoryIndex].likes,
}
```
NOTE: Here I passed the value of likes that the repository already had.

#### *should be able to delete the repository*

In order for this test to pass, you must allow your delete route to delete a project, and when deleted, it must return an empty response, with status 204.

```js
app.delete("/repositories/:id", (request, response) => {

  const { id } = request.params;

  const repositoryIndex = repositories.findIndex(repository => repository.id == id);

  if(repositoryIndex < 0) {
    return response.status(400).send('Repository not found');
  }

  repositories.splice(repositoryIndex, 1);

  return response.status(204).send();

});
```

#### *should not be able to delete a repository that does not exist*

In order for this test to pass, you must validate in your delete route whether the repository id sent by the URL exists or not. If not, return an error with status 400.

```js
if(repositoryIndex < 0) {
  return response.status(400).send('Repository not found');  
}
```

#### *should be able to give a like to the repository*

In order for this test to pass, your application must allow a repository with the given id to receive likes. The value of likes must be increased by 1 for each request, and as the result, return a JSON containing the repository with the number of likes updated.

```js
app.post("/repositories/:id/like", (request, response) => {
 
  const { id } = request.params;

  const repository = repositories.find(repository => repository.id == id);

  if(!repository) {
    return response.status(400).send('Repository not found.');
  }

  repository.likes += 1;

  return response.json(repository);

});
```

#### *should not be able to like a repository that does not exist*

In order for this test to pass, you must validate in your like route whether the repository id sent by the URL exists or not. If not, return an error with status 400.

```js
if(!repository) {
  return response.status(400).send('Repository not found.');
}
```
