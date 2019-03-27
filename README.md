![Trellis Logo](https://cdn.trellisconnect.com/sdk/v1.1/js-sdk/assets/images/header.png)

Drop-in React-based modal for clients to easily access a user's current insurance information.

# API Docs

You can find a full list of Trellis endpoints and schemas here: [Trellis API Docs](https://trellisconnect.com/docs)

# Usage

1. Include our SDK on your page.
2. Configure a handler using `TrellisConnect.configure()` as shown in the example.
3. Call your handler's `open()` method, and Trellis will present a modal dialog enabling the user to connect his or her insurance account.
4. Have your `onSuccess` method pass the `accountId` to your application server, which can call Trellis API endpoints to retrieve information about that account. (Note: Your application server – not your web and mobile clients - should access the Trellis API because such access requires the use of your `TRELLIS_SECRET_KEY`, which should never be publicly disseminated.)

```
<script src="https://cdn.trellisconnect.com/sdk/v1.1/trellis-connect.js"></script>
<a href='#' id='openTrellisButton'>Open Trellis</a>
<script>
(function() {
        var handler = TrellisConnect.configure({
          // Your trellis API Client-Id
          client_id: '<TRELLIS_API_CLIENT_ID>',

          // onSuccess(accountId, metadata)
          // Called when TrellisConnect has completed retrieving policy information from the user.
          // The function is passed in an accountId and a metadata object. The accountId can be
          // used by application server, combined with your TRELLIS_SECRET_KEY to pull policy data.
          // The metadata contains summary information about the account connected.
          onSuccess: handleTrellisSuccess,

          // onFailure()
          // Called each time the user attempts to authenticate with their insurer and fails.
          onFailure: handleTrellisFailure,

          // onClose()
          // Called when the user closes the modal dialog -- either when they have
          // successfully loaded their policies (potentially after an onSuccess() call) or by
          // clicking the "X" button in the top right of the modal.
          onClose: handleTrellisClose,

          // A URL that is called asynchronously by the Trellis API when it has completed
          // pulling insurance data.
          webhook: 'https://api.myserver.com/trellisUpdate'
        });
        document.getElementById('openTrellisButton').onclick = handler.open;
})();
</script>
```
