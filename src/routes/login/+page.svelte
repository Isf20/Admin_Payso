<script>
  // @ts-nocheck

  let Email_admin = "";
  let Password_admin = "";
  let message = "";

  const Sigindata = async () => {
    console.log("login");
    console.log("Email_admin:", Email_admin);

    let config = {
      method: "Post",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        Email_admin: Email_admin,
        Password_admin: Password_admin,
      }),
    };
    var result = await fetch(`http://localhost:9999/api/loginAddmin`, config);
    var data = await result.json();
    console.log(data);

    if (data.length == 0 && data.Status != 200) {
      message = update.Message;
      console.log("User not found");
    } else {
      console.log("");
      window.location.href = "/useradmin";
    }
  };
</script>

<main>
  <div class="left-column">
    <img src="src/routes/img/BG2.jpg" alt="Image" />
  </div>
  <div class="right-wrapper">
    <div class="right-column">
      <h2>เข้าสู่ระบบ</h2>
      <p style="text-align: center; color: #686D76;">
        ยินดีต้อนรับเข้าสู่เว็บไซต์ Payso Shop
      </p>
      <form id="login-form">
        <div class="form-group">
          <label for="Email">Email</label>
          <input
            type="Email"
            id="Email"
            name="Email"
            required
            bind:value={Email_admin}
          />
        </div>
        <div class="form-group">
          <label for="password">Password</label>
          <input
            type="password"
            id="password"
            name="password"
            required
            bind:value={Password_admin}
          />
          {#if message}
            <p class="alert">{message}</p>
          {/if}
        </div>
        <button
          type="button"
          class="btn btn-primary btn-lg"
          on:click={() => Sigindata()}>เข้าสู่ระบบ</button
        >
      </form>
      <label for="check" style="margin-top: 10px; "
        ><input type="checkbox" id="check" />Remember me</label
      >
      <p style="text-align: center;">
        ยังไม่มีบัญชีใช่หรือไม่? <a href="/register">ลงทะเบียน</a>
      </p>
    </div>
  </div>
</main>

<style>
  body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    font-family: Arial, sans-serif;
  }
  .alert {
    color: red;
    align-items: center;
    font-size: 15px;
  }
  main {
    display: flex;
    width: 100%;
    height: 100vh;
  }
  .error {
    color: red;
  }

  .left-column,
  .right-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
  }

  .left-column {
    width: 70%;
    background-color: #e0e0e0;
    background-image: url("src/routes/img/BG2.jpg"); /* Set the background image */
    background-size: cover; /* Cover the entire column */
    background-position: center; /* Center the image */
  }

  .left-column img {
    display: none; /* Hide the img tag if you use background image */
  }

  .right-wrapper {
    background-color: #ffffff;
    display: flex;
    padding: 80px 0 50px 50px;
  }

  .right-column {
    background-color: #ffffff;
    padding: 100px;

    display: flex;
    flex-direction: column;
    width: 120%; /* Adjust the width of the login box to be wider */
    max-width: 600px; /* Set a max-width to prevent it from being too wide */
  }

  h2 {
    margin-bottom: 20px;
    text-align: center;
  }

  .form-group {
    margin-bottom: 15px;
    width: 100%; /* Make form groups take full width */
  }

  label {
    display: block;
    margin-bottom: 5px;
    text-align: left; /* Align labels to the left */
    width: 100%;
  }

  input {
    padding: 10px 30px;
    font-size: 16px;
    cursor: pointer;
    border-radius: 10px;
  }
  button {
    display: block;
    margin: 20px auto; /* Center the button horizontally with a vertical margin */
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    width: 100%;
    text-align: center; /* Center text horizontally */
    vertical-align: middle; /* Center text vertically */
  }

  a {
    color: #007bff;
    text-decoration: none;
  }

  a:hover {
    text-decoration: underline;
  }

  p {
    margin: 10px 0;
    width: 100%; /* Make paragraphs take full width */
    text-align: left; /* Align paragraphs to the left */
  }
</style>
