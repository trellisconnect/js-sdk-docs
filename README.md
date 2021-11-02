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
      client_id: "<API_CLIENT_ID>",

      // onSuccess(connectionId, metadata)
      onSuccess: handleTrellisSuccess,
    });
    document.getElementById("openTrellisButton").onclick = handler.open;
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
      client_id: "<API_CLIENT_ID>",

      // Optional Trellis identifier for your application.
      application_id: "<APPLICATION_ID>",

      // The user object allows you to provide additional information about the user to be appended
      // reports. All fields are optional.
      user: {
        // An identifier you determine and submit for the user
        client_user_id: "<CLIENT_USER_ID>",
      },

      // Set `closeConfirmation` to `false` to have users skip the confirmation dialog after clicking the close button.
      closeConfirmation: true,

      // OPTIONAL: Set true to skip qualification questions (e.g. "Do you remember your login?") prior to the credentials page.
      skipQualificationQuestions: false,

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
      //
      // Metadata:
      //   status: The point the user reached before exiting the Connect flow. One of the following values:
      //     * issuer_selection - User prompted to select or search for their current issuer
      //     * issuer_not_found - User searched for their issuer and Connect found no results
      //     * unsupported_issuer - User selected an insurer that Connect does not support
      //     * trellis_consent - User prompted to accept Trellis' terms and conditions
      //     * unqualified - The user does not have authentication setup with his or her insurer (e.g. has no login)
      //     * requires_credentials - User prompted to provide credentials for the selected issuer
      //     * requires_questions - User prompted to answer security questions
      //     * requires_selections - User prompted to answer multiple choice question(s)
      //     * requires_code - User prompted to provide a one-time passcode
      //     * choose_device - User prompted to select a device on which to receive a one-time passcode
      //     * existing_client - User selected you, the client, as their current issuer
      onClose: function (error, metadata) {
        // metadata = {
        //   trellisSessionId: 'f5d62241-6a9a-46cb-bae4-b4972d54fb58'
        //   status: 'existing_client',
        // }
      },

      // OPTIONAL: onEvent(eventName, metadata)
      // Called when certain events happen. Supported event names:
      // - OPEN
      //     - The user has opened the widget.
      // - SELECT_ISSUER
      //     - The user has selected their insurance company.
      // - SUBMIT_CREDENTIALS
      //     - The user has submitted the crendetials for their insurance login.
      // - SUBMIT_MFA
      //     - The user was prompted with multi-factor authentication and has submitted their code
      // - AUTH_COMPLETE
      //     - The user has finished login/authentication for an account.
      // - TRANSITION_VIEW
      //     - When the Widget transitions between views.
      // - CLOSE
      //     - The flow has been exited. Also calls `onClose` callback.
      // - ERROR
      //     - If an error happens during the flow.
      onEvent: handleEvent,
      // OPTIONAL: Element which you want the Trellis Widget appended to. When not specified, the Trellis Widget will append to the body.
      // We recommend you add the style `position: realtive` to the container element if you wish the iframe to be contained within the container element.
      // The iframe overlay is absolutely positioned and will otherwise span outside of the container.
      containerElement: document.querySelector(".custom-container"),
    });
    document.getElementById("openTrellisButton").onclick = handler.open;
  })();
</script>
```

### Removing JS SDK with destroy()

destroy() allows you to remove all DOM artifacts created by JS SDK. This function will fail if called when Trellis Widget is open.

```html
<script>
  // Create the Trellis handler
  var handler = TrellisConnect.configure({ client_id: "<API_CLIENT_ID>" });

  // Destroy handler
  handler.destroy();
</script>
```

# CHANGELOG

- 5/7/2021
  - Added `onEvent` api documentation
- 2/8/2021
  - Added metadata.status to onClose()
- 7/22/2020
  - Updated to use `connectionId` instead of `accountId`
- 4/28/2020
  - Added "destroy" documentation
- 4/07/2020
  - Update "onClose" handler to pass metadata that includes the trellisSesssionId.
- 5/10/2019
  - Renamed "key" to "client_id". The tokens remain the same, this only rename of the field in this SDK. There is an analogous rename in the [API Documentation](https://trellisconnect.com/docs) of the header from X-API-KEY to X-CLIENT-ID. This was done to avoid confusion with the old names of API KEY and API SECRET KEY.
