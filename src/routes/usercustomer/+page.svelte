<script>
  // @ts-nocheck

  /**
   * @type {any[]}
   */
  import Swal from "sweetalert2";
  import { onMount } from "svelte";

  let customer = [];

  ///////////////////////////////////////////////////////////////////////////////////

  async function fetchData() {
    try {
      const response = await fetch("http://localhost:9999/api/userallcustomer");
      if (!response.ok) {
        throw new Error("ไม่เชื่อต่อ " + response.statusText);
      }
      customer = await response.json();
    } catch (error) {
      console.error("ดึงข้อมูลไม่ได้", error);
    }
  }
  fetchData();
  ///////////////////////////////////////////////////////////////////////////////////

  let modal_create = false;
  async function Register() {
    const requestOptions = {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(formdata),
    };
    let url = `http://localhost:9999/api/registercustomer/create`;
    const response = await fetch(url, requestOptions);
    var data = await response.json();
    console.log(data);

    if (data == "COMPLETE") {
      console.log("data = ");
    } else {
      console.log("fail");
    }
  }

  ///////////////////////////////////////////////////////////////////////////

  async function Delete(id) {
    const requestOptions = {
      method: "DELETE",
      headers: {
        "Content-Type": "application/json",
      },
    };
    let url = `http://localhost:9999/api/deletecustomer?id_customer=${id}`;
    try {
      const response = await fetch(url, requestOptions);
      const datele = await response.json();
      if (response.ok) {
        Swal.fire({
          title: "สำเร็จ",
          text: "ลบข้อมูลสำเร็จแล้ว",
          icon: "success",
        }).then(() => {
          window.location.reload();
        });
      } else {
        Swal.fire({
          title: "Error",
          text: "Delete Fail!!",
          icon: "error",
        });
      }
    } catch (error) {
      console.error("Failed to delete user:", error);
      Swal.fire({
        title: "Error",
        text: "Delete Fail!!",
        icon: "error",
      });
    }
  }

  ///////////////////////////////////////////////////////////////////////////////////
  let modal_update = false;
  let message = "";
  let formdata = {
    Name_customer: "",
    Code_user: "",
    Address_customer: "",
    Email_customer: "",
    Password_customer: "",
    ConfirmPassword: "",
    Phone_customer: "",
    Status: false,
  };
  async function Update(userid) {
    const requestOptions = {
      method: "PUT",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(formdata),
    };
    let url = `http://localhost:9999/api/updatecustomer?id_customer=${userid}`;
    const response = await fetch(url, requestOptions);
    var update = await response.json();
    console.log(update);
    if (update.Status == 200){
      window.location.reload();
      modal_update = false; 
    }else{
      message = update.Message
    }
  }
  ////////////////////////////////////////////////
  let id = "";
  async function Getuserid(id) {
    const requestOptions = {
      method: "GET",
    };
    let url = `http://localhost:9999/api/useraidcustomer?id_customer=${id}`;
    const response = await fetch(url, requestOptions);
    customer = await response.json();
    console.log(customer);
    if (id) {
      Getuserid(id);
    } else {
      window.location.reload();
    }
  }

  /////////////////////////////////////////////////////
</script>

<body>
  <div class="top-bar">
    <div class="d-flex justify-content-between">
      <h1>Pay shop</h1>
      <form class="d-flex">
        <input
          class="form-control me-2"
          type="search"
          placeholder="Search"
          aria-label="Search"
        />
        <button class="btn btn-success" type="submit">Search</button>
      </form>
    </div>
  </div>

  <div class="container">
    <nav>
      <ul>
        <li><a href="/useradmin">ผู้ดูแลระบบ</a></li>
        <li><a href="/userseller">ผู้ขาย</a></li>
        <li><a href="/usercustomer">ลูกค้า</a></li>
      </ul>
    </nav>
    <div class="container">
      <div class="teble-head" style="margin-top: 90px;">
        <div class="table-header">
          <h1 class="text">ลูกค้า</h1>
          <button
            class="btn btn-primary px-4"
            on:click={() => {
              modal_create = true;
            }}>เพิ่มลูกค้า</button
          >
        </div>
        <form class="d-flex search-form">
          <input
            class="form-control me-2"
            type="search"
            placeholder="Search"
            aria-label="Search"
            bind:value={id}
          />
          <button
            class="btncc btn-success"
            type="submit"
            style="margin-left: 10px;"
            on:click={() => Getuserid(id)}
          >
            <label for="submit">
              <span class="material-symbols-outlined">ค้นหา</span>
            </label>
          </button>
          <button
            class="btncc btn-success"
            type="submit"
            style="margin-left: 10px;"
            on:click={() => Getuserid()}
          >
            <label for="submit" >
              <span class="material-symbols-outlined">ล้าง</span>
            </label>
          </button>
        </form>
      </div>
      <table class="table" style="width: 100%; margin-top: 10px;">
        <thead class="table-primary">
          <tr>
            <th>#</th>
            <th>รหัสสมาชิก</th>
            <th>ชื่อ - นามสกุล</th>
            <th>อีเมล</th>
            <th>เบอร์โทรศัทพ์</th>
            <th>สถานะ</th>
            <th>Tilte</th>
          </tr>
        </thead>
        <tbody>
          <!-- Table rows -->
          {#each customer as user}
            <tr>
              <td>{user.Id_customer}</td>
              <td>{user.Code_user}</td>
              <td>{user.Name_customer} </td>
              <td>{user.Email_customer}</td>
              <td>{user.Phone_customer}</td>
              {#if user.Status}
                <div class="status-green">ใช้งาน</div>
              {:else}
                <div class="status-red">ปิดการใช้งาน</div>
              {/if}
              <td>
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <!-- svelte-ignore missing-declaration -->
                <!-- svelte-ignore a11y-click-events-have-key-events -->
                <svg
                  on:click={() => {
                    modal_update = true;
                    formdata.Id_customer = user.Id_customer;
                    formdata.Code_user = user.Code_user;
                    formdata.Name_customer = user.Name_customer;
                    formdata.Address_customer = user.Address_customer;
                    formdata.Email_customer = user.Email_customer;
                    formdata.Password_customer = formdata.Password_customer;
                    formdata.ConfirmPassword = formdata.ConfirmPassword;
                    formdata.Phone_customer = user.Phone_customer;
                    formdata.Status = formdata.Status;
                  }}
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-pencil-square mx-1"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"
                  />
                  <path
                    fill-rule="evenodd"
                    d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5z"
                  />
                </svg>
                <!-- svelte-ignore a11y-no-static-element-interactions -->
                <!-- svelte-ignore a11y-click-events-have-key-events -->
                <svg
                  on:click={() => {
                    Swal.fire({
                      title: "ต้องการลบข้อมูล",
                      text: "ยืนยันการลบข้อมูลนี้หรือไม่ ?",
                      icon: "warning",
                      showDenyButton: true,
                      denyButtonText: "ยกเลิก",
                      confirmButtonText: "ตกลง",
                    }).then((response) => {
                      if (response.isConfirmed) {
                        Delete(user.Id_customer);
                      }
                    });
                  }}
                  xmlns="http://www.w3.org/2000/svg"
                  width="16"
                  height="16"
                  fill="currentColor"
                  class="bi bi-trash mx-1"
                  viewBox="0 0 16 16"
                >
                  <path
                    d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5m3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0z"
                  />
                  <path
                    d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h3.5a1 1 0 0 1 1 1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4zM2.5 3h11V2h-11z"
                  />
                </svg>
              </td>
            </tr>
          {/each}
        </tbody>
      </table>
    </div>
  </div>
</body>
{#if modal_create}
  <div class="modal">
    <div class="modal-content">
      <div class="d-flex justify-content-between">
        <h3>{modal_create ? "เพิ่มลูกค้า  " : ""}</h3>
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <span
          class="close"
          on:click={() => {
            modal_create = false;
          }}>&times;</span
        >
      </div>
      <span class="mb-2">ชื่อ - นามสกุล :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.Name_customer}
      />
      <span class="mb-2">รหัสสมาชิก :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.Code_user}
      />

      <span class="mb-2">ที่อยู่ :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.Address_customer}
      />
      <span class="mb-2">เบอร์โทรศัทพ์ :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.Phone_customer}
        maxlength="10"
      />
      <span class="mb-2">อีเมล :</span>
      <input
        type="Email"
        class="form-control mb-2"
        bind:value={formdata.Email_customer}
      />
      <span class="mb-2">รหัสผ่าน :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.Password_customer}
      />
      <span class="mb-2">ยืนยันรหัสผ่าน :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.ConfirmPassword}
      />
      {#if message}
        <p class="alert">{message}</p>
      {/if}
      <div class="d-flex justify-content-end">
        <button
          type="button"
          class="btn btn-secondary px-4"
          on:click={() => {
            modal_create = false;
          }}>Cancel</button
        >
        <!-- svelte-ignore missing-declaration -->
        <button
          type="button	"
          class="btn btn-primary px-4 ms-3"
          on:click={() => {
            if (modal_create) {
              Register().then(() => {
                window.location.reload();
                modal_create = false;
              });
            }
          }}>Save</button
        >
      </div>
    </div>
  </div>
{/if}

{#if modal_update}
  <div class="modal">
    <div class="modal-content">
      <div class="d-flex justify-content-between">
        <h3>{modal_update ? "ลูกค้า" : ""}</h3>
        <span
          class="close"
          on:click={() => {
            modal_update = false;
          }}>&times;</span
        >
      </div>
      <span class="mb-2">ชื่อ - นามสกุล :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.Name_customer}
      />

      <span class="mb-2">ที่อยู่ :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.Address_customer}
      />
      <span class="mb-2">เบอร์โทรศัทพ์ :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.Phone_customer}
        maxlength="10"
      />
      <span class="mb-2">อีเมล :</span>
      <input
        type="Email"
        class="form-control mb-2"
        bind:value={formdata.Email_customer}
      />
      <span class="mb-2">รหัสผ่าน :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.Password_customer}
      />
      <span class="mb-2">ยืนยันรหัสผ่าน :</span>
      <input
        type="text"
        class="form-control mb-2"
        bind:value={formdata.ConfirmPassword}
      />
      <div class="toggle-switch">
        <span class="mb-2">ใช้งาน : </span>
        <input
          type="checkbox"
          id="toggle"
          class="toggle-input"
          bind:checked={formdata.Status}
        />
        <label for="toggle" class="toggle-label"></label>
      </div>
      {#if message}
        <p class="alert">{message}</p>
      {/if}
      <div class="d-flex justify-content-end">
        <button
          type="button"
          class="btn btn-secondary px-4"
          on:click={() => {
            modal_update = false;
          }}>Cancel</button
        >
        <!-- svelte-ignore missing-declaration -->
        <button
          type="button	"
          class="btn btn-primary px-4 ms-3"
          on:click={() => {
            if (modal_update) {
              Update();
              // window.location.reload();
              // modal_update = false;
            }
          }}>Save</button
        >
      </div>
    </div>
  </div>
{/if}

<style>
  body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
  }
  .alert {
    color: red;
    align-items: center;
    font-size: 15px;
  }

  .container {
    height: calc(100vh - 50px); /* Adjust height according to header size */
    max-width: 1580px;
    margin-left: 140px;
  }
  .btncc {
    background-color: #007bff;
    color: white;
    border: none;
    padding: 10px 30px 10px 20px;
    border-radius: 4px;
    cursor: pointer;
  }
  .top-bar {
    background-color: #cdd1d4;
    padding: 10px 20px;
    width: 100%;
    z-index: 1000; /* Ensure it's above other content */
    position: fixed; /* Fixed position at the top */
    top: 0;
    left: 0;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1); /* Drop shadow */
  }
  .top-bar h1 {
    color: black;
    margin-right: auto; /* ให้องค์ประกอบต่อไปอยู่ทางขวาของ h1 */
  }
  .search-form {
    display: flex;
    align-items: center;
  }
  .form-control {
    margin-right: 10px; /* ระยะห่างระหว่าง input และ button */
  }
  nav {
    background-color: #f8f9fa;
    width: 250px;
    padding: 20px;
    box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
    box-sizing: border-box;
    position: fixed;
    top: 60px; /* ระยะห่างด้านบนจาก top-bar */
    bottom: 0;
    left: 0;
    overflow-y: auto;
  }
  nav ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }
  nav li {
    margin: 15px 0;
  }
  nav a {
    color: #007bff;
    text-decoration: none;
    display: block;
    padding: 10px;
    border-radius: 4px;
  }
  nav a:hover {
    background-color: #007bff;
    color: white;
  }

  .teble-head {
    display: flex;
    flex-direction: column;
  }

  .text {
    color: #01204e;
    padding: 15px;
  }

  .table-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
  }

  .btn-primary {
    background-color: #007bff;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 4px;
    cursor: pointer;
  }

  .btn-primary:hover {
    background-color: #0056b3;
  }

  .search-form {
    width: 400px;
    display: flex;
    align-items: center;
  }

  .form-control {
    width: 200px;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin-right: 10px;
  }
  .status-green {
    display: inline-block;
    margin-top: 10px;
    padding: 5px 10px;
    background-color: #66bb6a;
    color: white;
    border-radius: 10px;
  }

  .status-red {
    display: inline-block;
    margin-top: 10px;
    padding: 5px 10px;
    background-color: #ef5350;
    color: white;
    border-radius: 10px;
  }

  .modal {
    display: flex;
    align-items: center;
    justify-content: center;
    position: fixed;
    z-index: 9999;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
  }

  .modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    width: 400px; /* Adjust width as needed */
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  }

  /* Close button styles */
  .close {
    cursor: pointer;
    font-size: 24px;
    font-weight: bold;
    color: #aaaaaa;
  }

  .close:hover,
  .close:focus {
    color: #000000;
    text-decoration: none;
    cursor: pointer;
  }

  /* Form input styles */
  .form-control {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  /* Toggle switch styles */
  .toggle-switch {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }

  .toggle-input {
    display: none;
  }

  .toggle-label {
    position: relative;
    display: inline-block;
    width: 40px;
    height: 21px;
    background-color: #ccc;
    left: 5px;
    border-radius: 20px;
    cursor: pointer;
  }

  .toggle-label:before {
    content: "";
    position: absolute;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background-color: white;
    left: 2px;
    top: 2px;
    transition: 0.3s;
  }

  .toggle-input:checked + .toggle-label {
    background-color: #66bb6a;
  }

  .toggle-input:checked + .toggle-label:before {
    transform: translateX(20px);
  }

  /* Button styles */
  .btn {
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .btn-secondary {
    background-color: #6c757d;
    color: white;
  }

  .btn-secondary:hover {
    background-color: #5a6268;
  }

  .btn-primary {
    background-color: #007bff;
    color: white;
  }

  .btn-primary:hover {
    background-color: #0056b3;
  }
</style>
