<!-- Javascript -->

<script>
  import { onMount, onDestroy } from "svelte";
  import { auth, db } from "../lib-firebase/firebase";
  import { getDoc, doc, setDoc } from "firebase/firestore";
  import { authStore } from "../store/store";
  import "../app.css";

  const nonAuthRoutes = ["/", "product"];

  let unsubscribe; // Declare the unsubscribe function at the component level

  onMount(() => {
    unsubscribe = auth.onAuthStateChanged(async (user) => {
      const currentPath = window.location.pathname;

      // if user doesn't exist or we are on unauthorize route, take us back to log in
      if (!user && !nonAuthRoutes.includes(currentPath)) {
        window.location.href = "/";
        return;
      }

      if (!user) {
        return;
      }

      // if user exists and we are at login then change location to dashboard
      if (user && currentPath === "/") {
        window.location.href = "/dashboard";
        return;
      }

      // var assign for userData from firestore
      let userDataForStore;

      // pull from firestore
      const docRef = doc(db, "users", user.uid);
      const docSnap = await getDoc(docRef);

      if (!docSnap.exists()) {
        // set user if doesnt exist
        const userRef = doc(db, "users", user.uid);

        // set userObject data with assigned var userData
        userDataForStore = {
          email: user.email,
          todos: [],
        };

        await setDoc(userRef, userDataForStore, { merge: true });
      } else {
        // if user exists pull data for user from store
        const userData = docSnap.data();

        // assign user data from store to variable
        userDataForStore = userData;
      }
      // update our authStore for firebase
      authStore.update((curr) => {
        return {
          ...curr,
          user,
          data: userDataForStore,
          loading: false,
        };
      });
    });
  });

  onDestroy(() => {
    if (unsubscribe) {
      unsubscribe(); // Invoke the unsubscribe function when the component is being unmounted
    }
  });
</script>

<!-- HTML -->
<div class="layoutContainer">
  <slot />
</div>

<!-- CSS -->
<style>
  .layoutContainer {
    min-height: 100vh;
    background: linear-gradient(to right, #000428, #000046);
    color: white;
    position: relative;
    display: flex;
    flex-direction: column;
  }
</style>
