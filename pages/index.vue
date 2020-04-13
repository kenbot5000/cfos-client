<template>
  <body>
    <div id="main" class="text-center">
      <div id="login-form" class="m-auto">
        <h2 class="h2 mb-4">Login to CFOS</h2>
        <form class="form-signin">
          <input type="text" id="txt-username" name="username" class="form-control" placeholder="Username">
          <input type="password" id="txt-password" name="password" class="form-control" placeholder="Password">
          <button type="submit" class="btn btn-primary mt-3">Log In</button>  
        </form>
      </div>
    </div>
  </body>
</template>

<script>
export default {

}
</script>

<style>
#main {
  height: 100vh;
  display: flex;
}

#login-form {
  max-width: 330px;
  width: 100%;
  display: block;
  flex-direction: column;
}

#txt-username {
  border-bottom-left-radius: 0px;
  border-bottom-right-radius: 0px;
}

#txt-password {
  border-top-left-radius: 0px;
  border-top-right-radius: 0px;
}
</style>