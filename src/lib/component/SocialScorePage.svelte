<script lang="ts">
  import { onMount } from "svelte";
  import Guage from "./Guage.svelte";

  //    Twitter user details management

  let twitterHandle: string = "@teisnilsson"; // Replace with actual Twitter handle later
  let profileImageUrl: string = "images/7926.png"; // Replace with actual image URL later

  // Function to update user info - for future real data fetching
  async function fetchTwitterData(twitterUsername: string) {
    // Placeholder for fetching real data from Twitter's API
    // For now, it's not implemented, so we're just using dummy data
    // Uncomment and implement the code below when you're ready to fetch real data
    // const response = await fetch(`https://api.twitter.com/user/${twitterUsername}`);
    // const data = await response.json();
    // twitterHandle = data.twitterHandle;
    // profileImageUrl = data.profileImageUrl;
  }

  // Placeholder for when you want to call real data function
  // onMount(() => {
  //     fetchTwitterData('realTwitterUsername');
  // });

  // Points counter functionality

  let actualPoints: number = 90; // The actual score of the account that can be made reactive for the functionality of the social score
  let displayPoints: number = 0;
  let percentage: number = 100; // And percentage is a number as well

  let walletAddress: string = "0x1234567890abcdef1234567890abcdef12345678"; // Example wallet address which can be tied to an api call or something
  let displayAddress: string = `${walletAddress.substring(0, 5)}...${walletAddress.substring(walletAddress.length - 5)}`;

  const maxPoints: number = 100;

  let dashOffset: number; // Declare dashOffset as a number

  const updateDisplay = () => {
    displayPoints = Math.min(displayPoints, actualPoints);
    console.log(`Dash Offset: ${dashOffset}, Display Points: ${displayPoints}`);
  };

  onMount(() => {
    updateDisplay();
    // Set initial SVG width based on client size
    const interval = setInterval(() => {
      if (displayPoints < actualPoints) {
        displayPoints++;
        updateDisplay();
      } else {
        clearInterval(interval);
      }
    }, 10);

    // // Add resize event listener within onMount
    // window.addEventListener("resize", () => {
    //   svgWidth = document.querySelector("svg").clientWidth;
    //   updateGauge();
    // });
  });

  // Role handler

  let roles: { name: string; color: string }[] = [
    { name: "Neutronium OG", color: "#008CFE" },
    { name: "Greed OG", color: "#00FEC9" },
    // Add more roles as needed
  ];

  // Function to add a new role
  function addRole(name: string, color: string) {
    roles = [...roles, { name, color }];
  }
</script>

<div class="outerBox">
  <div class="innerContent">
    <div class="leftSection">
      <div class="profileBox">
        <img
          src={profileImageUrl}
          alt="Profile image of {twitterHandle}"
          class="profilePicture"
        />
        <p class="twitterHandle">{twitterHandle}</p>
      </div>
      <!-- Any other user-related content can go here -->
    </div>

    <div class="rightSection">
      <div class="scoreTitleBox">Social Greed Score</div>
      <!-- New box with text -->

      <div class="box">
        <Guage />

        <div class="counter">
          <span class="score"
            ><h2>{displayPoints} Points</h2>
            <p>You are in the top {percentage}%.</p></span
          >
        </div>
      </div>
      <div class="walletAddress">{displayAddress}</div>
      <div class="rolesBox">
        {#each roles as role}
          <div class="role">
            <span class="dot" style="background-color: {role.color};"></span>
            {role.name}
          </div>
        {/each}
      </div>
    </div>
  </div>
</div>
