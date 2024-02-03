<!-- Javascript -->

<script>
  import { onMount, onDestroy } from "svelte";
  import { auth, db } from "../lib-firebase/firebase";
  import { getDoc, doc, setDoc } from "firebase/firestore";
  import { authStore } from "../store/store";

  const nonAuthRoutes = ["/", "product"];

  let unsubscribe; // Declare the unsubscribe function at the component level

  onMount(() => {
    console.log("Mounting");

    unsubscribe = auth.onAuthStateChanged(async (user) => {
      const currentPath = window.location.pathname;

      if (!user && !nonAuthRoutes.includes(currentPath)) {
        window.location.href = "/";
        return;
      }
      if (user && currentPath === "/") {
        window.location.href = "/dashboard";
        return;
      }

      if (!user) {
        return;
      }

      let userDataForStore;
      // pull from firestore
      const docRef = doc(db, "users", user.uid);
      const docSnap = await getDoc(docRef);
      // set user if doesnt exist
      if (!docSnap.exists()) {
        const userRef = doc(db, "user", user.uid);
        // set userObject data
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
      authStore.update((curr) => {
        return {
          ...curr,
          user,
          data: userDataForStore,
          loading: true,
        };
      });
    });
  });

  onDestroy(() => {
    console.log("Unmounting");
    if (unsubscribe) {
      unsubscribe(); // Invoke the unsubscribe function when the component is being unmounted
    }
  });
</script>

<!-- <script>
  import { onMount } from "svelte";
  import { auth, db } from "../lib-firebase/firebase";
  import { getDoc, doc, setDoc } from "firebase/firestore";
  import { authStore } from "../store/store";

  const nonAuthRoutes = ["/", "product"];

  onMount(() => {
    console.log("Mounting");

    const unsubscribe = auth.onAuthStateChanged(async (user) => {
      const currentPath = window.location.pathname;

      if (!user && !nonAuthRoutes.includes(currentPath)) {
        window.location.href = "/";
        return;
      }
      if (user && currentPath === "/") {
        window.location.href = "/dashboard";
        return;
      }

      if (!user) {
        return;
      }

      let userDataForStore;
      // pull from firestore
      const docRef = doc(db, "users", user.uid);
      const docSnap = await getDoc(docRef);
      // set user if doesnt exist
      if (!docSnap.exists()) {
        const userRef = doc(db, "user", user.uid);
        // set userObject data
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
</script> -->

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
