
# react-native-square-pos

## Getting started

### Installation

1. `npm install react-native-square-pos --save` or `yarn add react-native-square-pos`
2. `react-native link`

### iOS Installation

1. Add `RNSquarePos` to your Podfile: `pod 'RNSquarePos', :path => '../node_modules/react-native-square-pos'`, and run `pod install`
2. In the ["Getting Started" section from Square Docs](https://github.com/square/SquarePointOfSaleSDK-iOS/tree/fcb44143c9b199f62f9feb61e98a51516e0c28a3#update-your-infoplist), complete the following sections: "Update your Info.plist", and "Register your app with Square".  **Take note of the _URL Scheme_ and the _Application ID_**.
3. Make sure you've modified [`AppDelegate.m` to include `RCTLinking`](https://facebook.github.io/react-native/docs/linking).

### Android Installation

1. Follow ["Step 2: Register your application" in Square Android docs](https://docs.connect.squareup.com/payments/pos/setup-android#step-2-register-your-application). 
2. **Take note of your _Application ID_**  (this is found in the Square Developer dashboard, under "Credentials")


## Usage

Import the package

```javascript
import SquarePOS from 'react-native-square-pos'
```

Configure the package

```javascript
SquarePOS.configure({
  applicationId: 'Your Square application ID',
  callbackUrl: 'yourUrlScheme://some-unique-path'
})
```

Make a transaction

```javascript
const amountInCents = 100
const currency = 'CAD' // 🇨🇦
const options = {
  tenderTypes: [
    'CARD',
    'CARD_ON_FILE',
    'CASH',
    'OTHER'
  ],
  note: 'This note shows up on the transaction',
  locationId: 'Optionally pass location Id', // only on iOS at the moment
}
SquarePOS.transaction(amountInCents, currency, options)
  .then((result) => {
      // the transaction was successful
      const { transactionId, clientTransactionId } = result
  })
  .catch((err) => {
      // the transaction failed. 
      // see error codes below.
      const { errorCode } = err
  })
```

### Errors

There are a number of different error codes, which may differ on Android / iOS.  Eventually, an exhaustive list should be compiled here.  For now, here are the important ones:

- `CANNOT_OPEN_SQUARE`: The user doesn't have the Square POS app installed
- `NOT_LOGGED_IN`: The user isn't logged into a Square account
- `USER_NOT_ACTIVE`: Not totally sure what this does
- `PAYMENT_CANCELLED`: The payment got cancelled from within the app
- `NO_NETWORK_CONNECTION`: No network connection on the phone
- `AMOUNT_TOO_SMALL`: `amountInCents` was too small (iOS only)
- `AMOUNT_TOO_LARGE`: `amountInCents` was too large (iOS only)
- `INVALID_REQUEST`: Square Point-of-Sale couldn't process the request (because of malformed data or other) (Android only, check `err.squareResponse` for more information)

Additionally, because this is a young project:

- `UNKNOWN_ANDROID_ERROR`, if the response from Square Android SDK if the response wasn't handled by this package.  On Android, you'll have access to `err.squareResponse` to see the entire response data sent back from Square Point-of-Sale.
- `UNKNOWN_IOS_ERROR`, if the response from Square iOS SDK wasn't handled by this package.



