<template>
  <div id="wrapper">
    <sidebar-component></sidebar-component>
    <div id="content-wrapper" class="d-flex flex-column">
      <div id="content">
        <navbar-component></navbar-component>

        <div class="container-fluid">
          <div class="row">
            <div class="col-lg-8">
              <div class="card shadow mb-4">
                <div class="card-header py-3">
                  <h6 class="m-0 font-weight-bold text-primary">
                    Edit Data Paket
                  </h6>
                </div>
                <div class="card-body">
                  <form @submit.prevent="edit">
                    <div class="form-group">
                      <div>
                        <label>Jenis Paket</label>
                      <input type="text" class="form-control" v-model="paket.jenis" readonly>
                    </div>
                      </div>
                    <div class="form-group">
                      <label>Harga</label>
                      <textarea
                        rows="4"
                        class="form-control"
                        v-model="paket.harga"
                      ></textarea>
                    </div>

                    <button type="submit" class="btn btn-success btn-block">
                      Simpan
                    </button>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <footer-component></footer-component>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      paket: {}
      //   jenis: '',
      //   harga: ''

    };
  },
  created() {
    var data = JSON.parse(this.$store.state.datauser)
        var role = data.role
    if (role == 'kasir' || role == 'owner') {
        this.$swal("Error","Anda tidak bisa mengakses halaman ini", "error")
        this.$router.push('/')
      }
    this.axios
      .get(
        `http://127.0.0.1:8000/api/paket/${this.$route.params.id_paket}`,
        {
          headers: { Authorization: "Bearer " + this.$store.state.token }
        }
      )
      .then((res) => {
        this.paket = res.data;
      })
        .catch(err => console.log(err))
  },
  methods: {
    edit() {
      this.axios
        .put(
          `http://127.0.0.1:8000/api/paket/${this.$route.params.id_paket}`,
          this.paket,
          { headers: { Authorization: "Bearer " + this.$store.state.token } }
        )
        .then((res) => {
          this.$swal("Sukses", res.data.message, "success")
          this.$router.push("/paket");
        })
        .catch(err => console.log(err));
    },
  },
};
</script>