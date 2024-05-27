# CINURU-ANALYTICS-SDK

The SDK tracks behaviour of user when he visits the webpage of customer and sends the generated data to the cinuru-database. The SDK is hosted on the cinuru-fileserver and will be referenced in `<script>`-tag of customer-website, which injects the Cinuru-SDK-code into the website-code.

### Content

`RouteTracker`:
The SDK automatically tracks all routes, the user visits when browsing through the customers-website.

`Methods`:
In addition, the SDK adds a `cinuruSdk`-object to the window-object of the customer-website containing several new methods. The methods help to send additional and more specific data regarding the user behaviour to the cinuru-database. The methods have to be implemented and called manually within the belonging parts of the code of the website.

`Note:` When a new route is taken or a method of the `cinuruSdk`-methods is called, the generated data will automatically be sent to the cinuru-api.

### Implementation

1.) make sure CORS is allowed for the url of the customer in order for the cinuru-api to be called from the customers website
2.) add the following script tag to the header of your website

<script src='http://static.cinuru.com/public/website-sdk/sdk.js'></script>

### Methods

## recordTrailerPlay

After a user watched a trailer the interaction should be recorded. Needed Data: the data of the movie, the watch-duration and the total-duration of the video. The integrator should make sure, this method is called, when hitting the Stopbutton or leaving the page. If for any reasons it is not possible to record the interaction after the trailer has been watched, the interaction should be recorded right at the beginning. in this case pass 'elapsedSeconds': null.

Method-syntax:
`recordTrailerStop = (data: {externalMovieId: string; trailerId?: string; elapsedSeconds: string; totalSeconds: string;}) => {}`

Argument-example:
{
externalMovieId: 'movieXYZ_1234567',
trailerId: 'https://www.youtube.com/watch?v=zSWdZVtXT7E',
elapsedSeconds: '45',
totalSeconds: '87'
}

## recordTicketInterest

When user opens the ticketOption for a showtime, the data of the movie, showtimeDatetime and the customers showtimeId is immediately sent to the cinuru-database. Customer should make sure, this method is called, when hitting the Buy-Ticket-Button.

Method-syntax:
`recordTicketInterest = (data: {externalMovieId: string; showtimeDate: Date; showtimeId: string;}) => {}`

Argument-example:
{
externalMovieId: 'movieXYZ_1234567',
showtimeDate: "2020-10-05T14:48:00.000Z",
showtimeId: 'showtimeIdXYZ_1234567',
}

## recordTicketPurchase

When user buys a ticket, the data of the movie, showtimeDatetime and the customers showtimeId is immediately sent to the cinuru-database. Customer should make sure, this method is called, when user buys a ticket.

Method-syntax:
`recordTicketPurchase = (data: {externalMovieId: string; showtimeDate: Date; showtimeId: string;}) => {}`

Argument-example:
{
externalMovieId: 'movieXYZ_1234567',
showtimeDate: "2020-10-05T14:48:00.000Z",
showtimeId: 'showtimeIdXYZ_1234567',
}

## recordMovieDetailOpening

When user opens a movie-detail-page, the data of the movie is immediately sent to the cinuru-database. Customer should make sure, this method is called, when user browses a movie-detail-page.

Method-syntax:
`recordMovieDetailOpening = (data: {externalMovieId: string;}) => {}`

Argument-example:
{
externalMovieId: 'movieXYZ_1234567',
}

## recordMovieDetailClosing

When user closes a movie-detail-page, the data of the movie is immediately sent to the cinuru-database. Customer should make sure, this method is called, when user closes a movie-detail-page.

Method-syntax:
`recordMovieDetailClosing = (data: {externalMovieId: string; elapsedSeconds: string;}) => {}`

Argument-example:
{
externalMovieId: 'movieXYZ_1234567',
elapsedSeconds: '32';
}

## recordUserLogin

When user logs into the website, the cinemaUserId and user-email is immediately sent to the cinuru-database. Customer should make sure, this method is called, when user logs into website.

Method-syntax:
`recordUserLogin = (data: { email: string; userId: string }) => {}`

Argument-example:
{
userId: 'userXYZ_1234567',
email: 'userXYZ@testprovider.com';
}
