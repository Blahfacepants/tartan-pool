<svelte:head>
  <script src="https://accounts.google.com/gsi/client" async defer></script>
</svelte:head>


<script>
// @ts-nocheck

  import {profile, name} from "../../stores.js";
  import {onMount} from 'svelte';
  import jwt_decode from "jwt-decode";
  function onSignIn(decodedToken) {
      profile.set(decodedToken);
      name.set(decodedToken.name);
  };
  function handleCredentialResponse(response) {
    console.log("Encoded JWT ID token: " + JSON.stringify(response));
    onSignIn(jwt_decode(response.credential));
  }
  onMount(() => {
    google.accounts.id.initialize({
      client_id: "195033454694-otr43rcqlvgivd3gt373sb3hvl3slg6r.apps.googleusercontent.com",
      callback: handleCredentialResponse
    });
    google.accounts.id.renderButton(
      document.getElementById("buttonDiv"),
      { theme: "outline", size: "large" }  // customization attributes
    );
  });
</script>

<div id="buttonDiv"></div> 

