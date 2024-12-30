# Trello-API-PostMan

## Description
This Postman collection provides a set of requests for interacting with the Trello API. It includes creating and managing boards, as well as handling lists and cards within those boards.

## Features
- Create and Manage Boards
- Retrieve All Boards
- Retrieve Single Board Details
- Create and Manage Lists
- Create and Manage Cards
- Move Cards Between Lists
- Delete Boards

## Table of Contents
1. [Description](#description)
2. [Features](#features)
3. [Files](#files)
    - [Trello API](#trello-api)
        - [Create a Board](#create-a-board)
        - [Get All Boards](#get-all-boards)
        - [Get Single Board](#get-single-board)
        - [Create To Do List](#create-to-do-list)
        - [Create Done List](#create-done-list)
        - [Create a card](#create-a-card)
        - [Move a card](#move-a-card)  
        - [Delete a board](#delete-a-board)
        - [Get deleted board](#get-deleted-board)            
4. [Running the Collection](#running-the-collection)
   - [Prerequisites](#prerequisites) 
   - [Execute the Collection](#execute-the-collection)
5. [License](#license) 
6. [Contact Information](#contact-information) 
7. [Happy Testing](#happy-testing)

## Files
All the files of the project

### Trello API
Includes a comprehensive set of requests to interact with the Trello API. These requests are designed to automate the workflow, allowing for seamless creation, management, and retrieval of boards, lists, and cards. The requests include:

- Creating a Board

- Retrieving All Boards

- Retrieving Single Board Details

- Creating a To-Do List

- Creating a DONE List

- Creating a Card

- Moving a Card Between Lists

- Deleting a Board

- Retrieving Deleted Board Details

#### Create a Board
This request creates a new board in Trello by sending a `POST` request to the `/1/boards/` endpoint. It includes the necessary query parameters to specify the board name, API key, and access token.

**Prerequest Script**:

```JavaScript
let boardNumber = pm.collectionVariables.get('boardNumber');

if (isNaN(boardNumber)) {
    boardNumber = 0
};

boardNumber ++
pm.collectionVariables.set('boardNumber', boardNumber);
```

*Explanation*:

- **Increment Board Number**: Retrieves the current board number from collection variables and checks if it's a valid number. If it is not (`NaN`), it initializes the board number to 0. Then, it increments the board number by one and updates the collection variable.

**Post-Response Script**:

```JavaScript
let response;

pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Board is Created Successfully", () => {
    response = pm.response.json();
    pm.expect(response.name).to.eql('postMan_Board' +' '+ pm.collectionVariables.get('boardNumber'));
    pm.expect(response.closed).to.be.false;
    pm.collectionVariables.set("board_id", response.id);
});
pm.test("Board is private", () => {
    pm.expect(response.prefs.permissionLevel).to.eql("private");
});
pm.test("Calendar is disabled", () => {
    pm.expect(response.prefs.switcherViews[2].enabled).to.be.false;
});
```

*Explanation*:

- **Status code is 200**: Verifies that the status code returned from the response is 200, indicating a successful request.

- **Board is Created Successfully**: Checks that the board is created with the expected name, is not closed, and stores the board ID in collection variables.

- **Board is private**: Ensures that the board's permission level is set to `private`.

- **Calendar is disabled**: Confirms that the calendar view is disabled for the created board.

#### Get All Boards
This request retrieves a list of all boards for the authenticated user by sending a `GET` request to the `/1/members/me/boards` endpoint, and verifies that the correct status code is returned.

**Post-Response Script**:

```JavaScript
const response = pm.response.json();

pm.test("Status code is 200", () => {
    pm.response.to.have.status(200);
});

existingBoardIds = response.map(board => board.id);
pm.collectionVariables.set('existingBoardIds', existingBoardIds);
```

*Explanation*:

- **Status code is 200**: Verifies that the status code returned from the response is 200, indicating a successful request.

- **Extract existing board IDs**: Parses the response to extract the IDs of all boards and stores them in a collection variable named `existingBoardIds`.

#### Get Single Board
This request retrieves details of a specific board by sending a `GET` request to the `/1/boards/:id` endpoint. The ID of the targeted board is provided as a path variable `:id`, and the key and token for authentication as query parameters.

**Post-Response Script**:

```JavaScript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

 *Explanation*:

- **Status code is 200**: Verifies that the status code returned from the response is 200, indicating a successful request.
