# CINURU ANALYTICS SDK

The SDK is used to record movie preferences while a user browses on the cinemas webpage.

### Definitions

#### Cinema

The cinema or group of cinemas to which the website belongs.

#### Integrator

The entity that builds the website for the cinema. If you are reading this documentation and you are not a cinuru employee, this is likely you.

#### User

This is the end-user, the person visiting the website, buying tickets and going to the movies.

### How to integrate the SDK into a Website

The integration is fairly easy: Add a `<script>` tag to the header of the website. This loads the sdk from the cinuru server and makes the SDKs methods available to your website.

Afterwards, call the appropriate methods to record interactions such as trailer plays or ticket purchases (explained in detail below).

### Content

`RouteTracker`:
If the user allowed analytics, the SDK automatically records all routes which the user visits when browsing through the customers-website.

`Methods`:
In addition, the SDK adds a `cinuruSdk`-object to the window-object of the customer-website containing several methods. The methods help to send additional and more specific data regarding the user behaviour to the cinuru-database. This could be, for example a trailer play, or a ticket purchase. The methods have to be called by the website when the corresponding actions occur.

### Implementation

1.) make sure CORS is allowed for the url of the customer in order for the cinuru-api to be called from the customers website
2.) add the following script tag to the header of your website

<script src='http://static.cinuru.com/public/website-sdk/sdk.js'></script>

### Methods

## recordConsent

This method is used to indicate wether a user has allowed analytics or not. It should be called from e.g. a cookie banner and the privacy settings of the website.

Only after the method recordConsent(true) has been called any other methods will have any effects.

If the user allowed analytics, this method creates a random uuid for the user and stores it in a cookie. The consent is stored in the cookie as well.

If the user disabled analytics (`recordConsent(false)`), the choice to reject analytics will be persisted in a cookie. If the user had allowed analytics before and a cookie with a user_id existed, it will be deleted. After calling `recordConsent(false)`, all other methods (e.g. recordTrailerPlay) will be disabled and have no effect.

```
// To allow analytics call
window.cinuruSdk.recordConsent(true);

// To disallow analytics call
window.cinuruSdk.recordConsent(false);
```

## recordTrailerPlay

After a user watched a trailer the interaction should be recorded. Needed Data: the data of the movie, the watch-duration and the total-duration of the video. The integrator should make sure, this method is called, when hitting the Stopbutton or leaving the page. If for any reasons it is not possible to record the interaction after the trailer has been watched, the interaction should be recorded right at the beginning. in this case pass 'elapsedSeconds': null.

Method-syntax:

```js
recordTrailerPlay = (data: {
	movieId: string,
	trailerId?: string,
	elapsedSeconds: number | null,
	totalSeconds: number,
}) => void;
```

How to call it:

```js
window.cinuruSdk.recordTrailerPlay({
	movieId: 'movieXYZ_1234567',
	trailerId: 'https://www.youtube.com/watch?v=abcdefg',
	elapsedSeconds: 45,
	totalSeconds: 87,
});
```

## recordTicketInterest

This method should be called when user opens the ticketing flow for a showtime (e.g. seat plan). It needs the data of the movie, showtimeDatetime and the customers showtimeId.

Method-syntax:

```js
recordTicketInterest = (data: {movieId: string; showtimeDate: Date; showtimeId: string;}) => void
```

How to call it:

```js
window.cinuruSdk.recordTicketInterest({
	movieId: 'movieXYZ_1234567',
	showtimeDate: '2020-10-05T14:48:00.000Z',
	showtimeId: 'showtimeIdXYZ_1234567',
});
```

## recordTicketPurchase

Use this method to record that a user has successfully bought a ticket. If this is not possible, because ticketing is done via an external online ticketing provider, please make sure to call at least `recordTicketInterest` before navigating to the online ticketing provider.

Method-syntax:

```js
recordTicketPurchase = (data: {movieId: string; showtimeDate: Date; showtimeId: string;}) => void;
```

How to call it:

```js
window.cinuruSdk.recordTicketPurchase({
movieId: 'movieXYZ_1234567',
showtimeDate: "2020-10-05T14:48:00.000Z",
showtimeId: 'showtimeIdXYZ_1234567',
}
```

## recordMovieDetailOpening

This method should be called when a user navigates to the details page of a movie.

Method-syntax:

```ts
recordMovieDetailOpening = (movieId: string) => void;
```

How to call it:

```js
window.cinuruSdk.recordMovieDetailOpening('movieXYZ_1234567');
```

## recordMovieDetailClosing

This method should be called once a user leaves a previously opened movie details page.

Method-syntax:

```ts
recordMovieDetailClosing = (movieId: string) => void;
```

How to call it:

```js
window.cinuruSdk.recordMovieDetailClosing('movieXYZ_1234567');
```

## recordUserLogin

This method is called, when user logs in.

Method-syntax:

```ts
recordUserLogin = (data: { email: string; userId: string }) => void;
```

How to call it:

```js
window.cinuruSdk.recordUserLogin({
cinemaUserId: 'userXYZ_1234567',
email: 'userXYZ@testprovider.com';
});
```
