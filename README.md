# Another Game

## Project overview
  Another Game is backoffice application to let users browse and manage game references, via an already existing REST API. The app is available at [this link](https://smaranza.me/dist/another-game.html/). Here's documentation, which explains developed features, known-bugs and improvomentes proposals.
  Code has been developed with a particolar focus on code scalability, maintainance and UI/UX accessibility.

## API Requests
Two API versions where provided. *Version 2* integrates all the calls of *Version 1* with an authorization method.
#### Manage Data

- **GET data/retrieve/all**: (version 1)
  > request is called as first, in order to provide content in the archive. Games catalogue grid is populated and updated each time data is being edited

- **GET data/retrieve/{id}**:  (version 1)
  > method is being called for demonstration at search click action (click on "search" button, in navbar). The search result is temporally provided only via console.log() for string **exact matches**. 

- **POST data/create**:  (version 1)
  >  request is called once the "New game" modal form is validated and submitted. Modal is accessible via "Add game" button at the top of the game catalogue.  Consequentially, game catalogue is being updated.

- **PUT data/update**:  (version 1)
  >  request is called once the "Edit game" modal form is validated and submitted. The modal can be opened via game entries "edit" button provided in eachrow/card. Consequentially, game catalogue is being updated at modal close.
  
- **DELETE data/delete/{id}**:  (version 1)
  >  Game entries can be deleted via "Edit game" modal form, once the "delete game" button is clicked and the next prompted alert is being confirmed. Consequentially, game catalogue is being updated at modal close.

- **POST credentials/authorize**  (version 2)
  >  Authorization call is being made at application loading.As a demonstration, the request is being made and visible in network

## UI/UX Features
UI/UX is being developed and arranged using the powerful tools provided by Vue and Bootstrap. Styling interpretations, that differ from the provided designs, were done design-wisely, with th intention to provide the user with the best available UI/UX. Some notable features are:
- Dynamic content modals
- Form validation with visual feedback
- Mobile media queries
- View mode toggle 
- pre-compiled forms
- hierarchy-wise buttons 
- Colorful tags for first-sight visual filtering
- etc...

## Futher improvements ideas
Some further improvements that I would recommend, would be:
- Data sorting and filtering
- Pre-loading placeholder 
- Theme toggle for dark/light mode
- Toast notification for successfull data calls
- etc...

## Known Bugs
No one is perfect. Here's a list of some missing features and known bugs to be fixed ASAP:
- "Invalid Date" as entry "release date", once the "edit game" modal is opened. Data is however won't be corrupted
- Search function provides ultra-basic logic for exact matching only (case sensitive, exact string matches)
- The only successful Version 2 API request is *credential/authorize*. However Bearer token is correctly fetched
