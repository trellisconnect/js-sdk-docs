![Trellis Logo](https://cdn.trellisconnect.com/sdk/v1.1/js-sdk/assets/images/header.png)

Easily access a user's insurance information using Trellis Connect.

## Instructions

1. Include the Trellis Connect JS SDK on your page.
2. Configure a handler using `TrellisConnect.configure()` as shown in the example.
3. Call your handler's `open()` method, and Trellis will present a widget enabling the user to connect his or her insurance account.
4. Use `connectionId` from the SDK's `onSuccess()` callback to access the user's insurance information. Client applications can use the connection's Temporary Access Key and server applications can use the Secret Key provided by your Trellis account manager.

## Basic Example

```html
<script src="https://cdn.trellisconnect.com/sdk/v1.1/trellis-connect.js"></script>
<a href="#" id="openTrellisButton">Open Trellis</a>
<script>
  (function () {
    var handler = TrellisConnect.configure({
      // Your Trellis Client-ID
      client_id: '<API_CLIENT_ID>',

      // onSuccess(connectionId, metadata)
      onSuccess: handleTrellisSuccess,
    });
    document.getElementById('openTrellisButton').onclick = handler.open;
  })();
</script>
```

## Advanced Features

```html
<script src="https://cdn.trellisconnect.com/sdk/v1.1/trellis-connect.js"></script>
<a href="#" id="openTrellisButton">Open Trellis</a>
<script>
  (function () {
    var handler = TrellisConnect.configure({
      // Your trellis API Client-Id
      client_id: '<API_CLIENT_ID>',

      // The `closeConfirmation` property controls the close confirmation screen
      // that appears when a users attempts to exit the flow. Setting to false
      // will close the widget immediately.
      closeConfirmation: true,

      // onSuccess(connectionId, metadata)
      // Called when TrellisConnect has completed retrieving policy information from the user.
      // The function is passed in an connectionId and a metadata object. The connectionId can be
      // used by application server, combined with your Trellis API-SECRET-KEY to pull policy data.
      // The metadata contains summary information about the connection.
      onSuccess: handleTrellisSuccess,

      // onFailure()
      // Called each time the user attempts to authenticate with their insurer and fails.
      onFailure: handleTrellisFailure,

      // onClose(error, metadata)
      // Called when the user closes the widget -- either when they have
      // successfully loaded their policies (potentially after an onSuccess() call) or by
      // clicking the "X" button in the top right of the widget.
      onClose: function (error, metadata) {
        // metadata = {
        //   trellisSessionId: 'f5d62241-6a9a-46cb-bae4-b4972d54fb58'
        // }
      },

      // onEvent(event, params)
      onEvent: handleEvent,
    });
    document.getElementById('openTrellisButton').onclick = handler.open;
  })();
</script>
```

### Removing JS SDK with destroy()

destroy() allows you to remove all DOM artifacts created by JS SDK. This function will fail if called when Trellis Widget is open.

```html
<script>
  // Create the Trellis handler
  var handler = TrellisConnect.configure({ client_id: '<API_CLIENT_ID>' });

  // Destroy handler
  handler.destroy();
</script>
```

# CHANGELOG

- 7/22/2020
  - Updated to use `connectionId` instead of `accountId`
- 4/28/2020
  - Added "destroy" documentation
- 4/07/2020
  - Update "onClose" handler to pass metadata that includes the trellisSesssionId.
- 5/10/2019
  - Renamed "key" to "client_id". The tokens remain the same, this only rename of the field in this SDK. There is an analogous rename in the [API Documentation](https://trellisconnect.com/docs) of the header from X-API-KEY to X-CLIENT-ID. This was done to avoid confusion with the old names of API KEY and API SECRET KEY.
