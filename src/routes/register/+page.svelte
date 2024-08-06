<script>
  // @ts-nocheck

  let Name_admin = "";
  let Code_admin = "";
  let Email_admin = "";
  let Password_admin = "";
  let Phone_admin = "";
  let message = "";

  let test = await fetch("http://localhost:9999/api/registeraddmin/create");
  test = await test.json();

  if (test.Data && test.Data.errors) {
    console.log(test.Data);
  }

  const Sigindata = async () => {
    console.log("regster");
    console.log("Email_admin:", Email_admin);

    let config = {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        Name_admin: Name_admin,
        Code_admin: Code_admin,
        Email_admin: Email_admin,
        Password_admin: Password_admin,
        ConfirmPassword : ConfirmPassword,
        Phone_admin: Phone_admin,
        Status:false,
      }),
    };

    var result = await fetch(
      `http://localhost:9999/api/registeraddmin/create`,
      config
    );

    var data = await result.json();
    console.log(data);

    if (data == "COMPLETE") {
      console.log("data = ");
      window.location.href = "/useradmin";
    } else {
      console.log("fail");
    }
  };
</script>

<main>
  <div class="left-column">
    <img src="src/routes/img/BG2.jpg" alt="Image" />
  </div>
  <div class="right-wrapper">
    <div class="right-column">
      <h3 style="text-align: center;">สร้างบัญชีผ้ใช้งาน</h3>
      <p style="text-align: center; color: #686D76;">
        สร้างบัญชีผู้ใช้กับเว็บของเรา
      </p>
      <form id="login-form">
        <div class="form-group">
          <label for="text">ชื่อ - นามสกุล</label>
          <input
            type="text"
            placeholder="Name"
            autocomplete="off"
            required
            bind:value={Name_admin}
          />
        </div>
        <div class="form-group">
          <label for="text">รหัสผู้ดูแล</label>
          <input
            type="text"
            placeholder="Name"
            autocomplete="off"
            required
            bind:value={Code_admin}
          />
        </div>
        <div class="form-group">
          <label for="email">อีเมล</label>
          <input
            type="email"
            placeholder="email"
            autocomplete="off"
            required
            bind:value={Email_admin}
          />
        </div>
        <div class="form-group">
          <label for="password">รหัสผ่าน</label>
          <input
            type="password"
            id="password"
            name="password"
            required
            bind:value={Password_admin}
          />
        </div>

        <div class="form-group">
          <label for="text">เบอร์โทรศัทพ์</label>
          <input
            type="text"
            placeholder="Phome"
            autocomplete="off"
            required
            bind:value={Phone_admin}
          />
        </div>
        {#if message}
          <p class="alert">{message}</p>
        {/if}
        <div class="input-summit">
          <button class="submit-btn" id="summit"
            ><label for="summit" on:click={() => Sigindata()}
              >สมัครสมาชิก
            </label></button
          >
        </div>

        <label for="check" style="margin-top: 10px; "
          ><input type="checkbox" id="check" />Remember me</label
        >
        <p style="text-align: center;">
          ยังมีบัญชีใช่หรือไม่? <a href="/login">เข้าสู่ระบบ</a>
        </p>
      </form>
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
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    width: 100%;
    text-align: center;
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
