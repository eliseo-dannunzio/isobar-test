# isobar-test

## Installation ##
 - Download the repo into a folder called `isobar-test`
 - `cd isobar-test`
 - `npm install` to ensure all NPM packages are up to date
 - `yarn run serve` to run the server
 - Open a browser and go to `localhost:8080`

## Notes ##

 - I was unable to get the fake authentication to work, so I have commented the functionality and the modal code out.
 - The modal code is not CSS formatted, given that I was unable to successfully get the fake authentication to work.
 - After a lot of trial and error, I found it difficult to add an index to the main lesson object, so I manually added one into the object. It's not the most desirable solution, but it does get the job done. Side note: Why an object with specific courses would not have a unique identifier as one of the properties by default is beyond me, that said, putting in an index was the only alternative in order ot make the transfer process workable.
