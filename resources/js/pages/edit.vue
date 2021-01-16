<template>
  <div id="app" class="">
    <div class="row">
      <div class="col-sm-3">
        <div class="card">
          <div class="card-body">
            <div class="text-uppercase text-bold">
              id selected: {{ selected }}
            </div>
            <!-- this form is to add new data or upload image -->
            <form class="form-horizontal">
              <div class="form-group row">
                <label for="inputName" class="col-sm-3 col-form-label"
                  >Name</label
                >
                <div class="col-sm-9">
                  <!-- here  is form that come frome data in script-->
                  <input
                    type="text"
                    v-model="form.name"
                    class="form-control"
                    :class="{ 'is-invalid': form.errors.has('name') }"
                  />
                  <has-error :form="form" field="name"></has-error>
                </div>
              </div>

              <div class="form-group row">
                <label for="inputSkills" class="col-sm-3 col-form-label"
                  >Avatar</label
                >
                <div class="col-sm-9">
                  <!-- this input is to upload new image or file -->
                  <!-- here  is form that come frome data in script-->
                  <input
                    type="file"
                    @change="uploadPhoto"
                    name="photo"
                    class="form-control"
                    :class="{ 'is-invalid': form.errors.has('photo') }"
                  />
                  <has-error :form="form" field="photo"></has-error>
                </div>
              </div>
              <!-- new upload image come here to show before add in table -->
              <div v-if="imageprevi" class="mt-3">
                <img
                  :src="imageprevi"
                  class="figure-img img-fluid rounded"
                  style="max-height: 100px"
                />
              </div>

              <div class="form-group row">
                <div class="offset-sm-2 col-sm-10">
                  <button
                    type="submit"
                    @click.prevent="profileUpload"
                    class="btn btn-primary"
                  >
                    Submit
                  </button>
                  
                </div>
              </div>
            </form>
            
          </div>
          <button class="btn btn-danger sel" @click="Delete(selected)">
                      Delete Select
                    </button>
        </div>
      </div>
      <div class="col-sm-9">
        <div class="card">
          <div class="card-body">
            <!-- <h5 class="card-title">Special title treatment</h5> -->
            <div v-if="books.length">
              <table class="table table-bordered">
                <tr>
                  <th>
                    
                      <div class="checkbox">
                      <input
                        class="flipswitch"
                        id="checkbox1"
                        type="checkbox"
                        v-model="selectAll"
                        @click="select"
                      />
                      <label for="checkbox1"></label>
                      
                      </div>

                      
                    
                        
                    <!-- <button class="btn btn-danger" @click="Delete(selected)">
                      Delete
                    </button> -->
                       
                  </th>
                  
                    <th>
                     
id         
                      </th>
                  <th>Name</th>
                  <th>Photo</th>
                  <th>Modify </th>
                </tr>
                <tr v-for="book in books" :key="book.id">
                  <td v-if="!book.id == 0">
                    <label class="form-checkbox inside">
                      <input
                        type="checkbox"
                        id="checkbox1"
                        :value="book.id"
                        v-model="selected"
                        @click="sel"
                      />
                      <label for="checkbox1"></label>
                    </label>
                  </td>

                  <td>{{ book.id }}</td>
                  <td>{{ book.name }}</td>
                  <td>
                    <!-- by this code we get image from file in framework according id that come frome Table -->
                    <img
                      :src="`img/public/${book.photo}`"
                      class="profile-user-img img-fluid img-circle"
                      style="height: 40px; width: 40px"
                    />
                  </td>

                  <td>
                    <!-- to edit or delete we need the below code to connect function -->
                    <!-- for edit we set just item because item come frome all inform of table -->
                    <a href="#" @click="editPhotoModal(book)"> edit </a>
                    |
                    <!-- for delete we set just item.id because item.id come frome all inform of table -->
                    <a href="#" @click="deletePhoto(book.id)"> delete </a>
                  </td>
                </tr>
              </table>
            </div>
            <h1 class="bg-warning text-center" v-else>No Record left!</h1>
          </div>
        </div>
      </div>
    </div>
    <div
      class="modal"
      id="addNew"
      tabindex="-1"
      role="dialog"
      aria-labelledby="addNewLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="addNewLabel">Update Photo</h5>

            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <!-- this fom is to update -->
          <form>
            <div class="modal-body">
              <div class="form-group">
                <!-- here is formm -->
                <input
                  v-model="formm.name"
                  type="text"
                  name="name"
                  placeholder="Name"
                  class="form-control"
                />
              </div>

              <div class="form-group row">
                <label for="inputSkills" class="col-sm-2 col-form-label"
                  >Update</label
                >
                <div class="col-sm-10" >
                  <input
                    type="file"
                    @change="uploadPh"
                    name="photo"
                    class="form-control"
                  />
                </div>
              </div>
              <div v-if="user.photo" class="mt-3" >
                <img
                  :src="`img/public/${user.photo}`"
                  class="figure-img img-fluid rounded"
                  style="max-height: 100px"
                />
              </div>
              <div v-if="image" class="mt-3 im" id="mybox" >
                <img 
                  :src="image"
                  class="figure-img img-fluid rounded i"
                  style="max-height: 100px;"
                />
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-danger" data-dismiss="modal">
                Close
              </button>
              <button
                type="submit"
                class="btn btn-primary"
                @click.prevent="UpdatePhoto(formm.id)"
              >
                Update
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      books: {},
      png: null,
      user_image: null,
      success: false,
      has_error: false,
      error: "",

      disabled: false,
      dataUrl: "",

      numbers: "",
      page: "",
      pag: "",
      tabledata: {},
      user: {},
      // in vue for use each of Form must definde tha Form. in vue in the Form insted of value add v-model to request .i used 2 Form in this Project one for add other for update & clone vform and install in node_modules
      // the below code is for update form
      formm: new Form({
        id: "",
        name: "",
        photo: "",
      }),
      // the below code is for Add form
      form: new Form({
        id: "",
        name: "",
        photo: "",
      }),
      imageprevi: null,
      image: null,
      selected: [],
      selectAll: false,
    };
  },
  
  mounted() {
    this.getResults();
  },
  methods: {
    uploadPhoto(e) {
      // upload new file or image do by this code
      let file = e.target.files[0];
      let reader = new FileReader();
      if (file["size"] < 2111775) {
        reader.onloadend = (file) => {
          this.form.photo = reader.result;
        };
        reader.readAsDataURL(file);
        reader.onload = (e) => {
          this.imageprevi = e.target.result;
        };
      } else {
        alert("File size can not be bigger than 2 MB");
      }
    },
    uploadPh(e) {
      //  upload updat file or image do by this code
      let file = e.target.files[0];
      let reader = new FileReader();
      if (file["size"] < 2111775) {
        reader.onloadend = (file) => {
          //console.log('RESULT', reader.result)
          this.formm.photo = reader.result;
        };
        reader.readAsDataURL(file);
        reader.onload = (e) => {
          this.image = e.target.result; // fadeOut()
          $(".im").show();
        };
      } else {
        alert("File size can not be bigger than 2 MB");
      }
    },
    //For getting Instant Uploaded Photo
    getPhoto() {
      let photo =
        this.form.photo.length > 100
          ? this.form.photo
          : "public/img/profile/" + this.form.photo;
      return photo;
    },
    //Insert Photo
    profileUpload() {
      // insert new file or image by this code
      this.form
        .post("http://localhost:8000/api/st")
        .then(() => {
          Toast.fire({
            icon: "success",
            title: "File uploaded successfully",
          });
          this.getResults();
        })
        .catch(() => {
          console.log("Error.....");
        });
    },
    //get Table data
    loadTableData(page) {
      if (typeof page === "undefined") {
        page = 1;
      }
      axios
        .get("http://localhost:8000/api/photo?page=" + page)
        .then(({ data }) => (this.tabledata = data))
        .catch(() => {
          console.log("Error...");
        });
    },
    // is here
    getResults(page) {
      // we must set varriable in where that get data from backaend

      if (typeof page == "undefined") {
        // i sayed to function if page undefined get currentHost of href
        // and if currentHost  == exampleURL(http://localhost:8000/edit#) page = 1 (the number of 1 is first page of data that get from backaend for paginate)
        var currentHost = window.location.href;
        var exampleURL = "http://localhost:8000/edit#";
        if (currentHost == exampleURL) {
          page = 1;
        } else {
          // for get number of page 1 set this function from js
          var numbers = currentHost.match(/\/edit#\/([0-9]+)/)[1];
          page = numbers;
        }
      }
      // according page 1 can get data from backaend
      axios
        .get("../photo?page=" + page)

        .then((response) => {
          this.books = response.data.data;
          window.history.pushState("", "", "/edit#/" + page);
        })
    },
    //Edit
    editPhotoModal(item) {
      // to edit must show data in modal and that do by this function according id
      this.formm.clear();
      this.formm.reset();
      axios
        .get("http://localhost:8000/api/show/" + item.id)
        .then((response) => {
          this.user = response.data;
          console.log(this.user.name);
        }); //  im
      $("#addNew").modal("show");

      //$(".im").css("display", "none");
      this.formm.fill(item);
    },
    UpdatePhoto(id) {
      // edit or update data by this function
      this.formm
        .post("http://localhost:8000/api/update/" + id, { id: this.formm.id })
        .then(() => {
          Toast.fire({
            icon: "success",
            title: "Photo updated successfully",
          });
          this.getResults();

          $("#addNew").modal("hide");
          $(".im").hide();
          $(".image").addClass("intro");
        })
        .catch(() => {
          console.log("Error.....");
        });
    },
    //Delete photo
    deletePhoto(id) {
      // delete data by this function
      Swal.fire({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!",
      }).then((result) => {
        if (result.value) {
          //Send Request to server
          this.form
            .post("../store_image/delete/" + id, {
              id: this.formm.id,
            })
            .then((response) => {
              Swal.fire("Deleted!", "Photo deleted successfully", "success");
              this.getResults();
            })
            .catch(() => {
              Swal.fire({
                icon: "error",
                title: "Oops...",
                text: "Something went wrong!",
                footer: "<a href>Why do I have this issue?</a>",
              });
            });
        }
      });
    },
    select() {
      this.selected = [];
      $(".sel").css('display' , 'inline');
      if (!this.selectAll) {
        for (let i in this.books) {
          this.selected.push(this.books[i].id);
          this.selectAll = false;
        }
      }
    },
    sel() {
      
        //$(".btn").css('color' , 'black');
        $(".sel").css('display' , 'inline');
        
      }
  ,

    //  Delete
    Delete(selected) {
      Swal.fire({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!",
      }).then((result) => {
        axios
          .post("http://localhost:8000/api/store_image/delete/" + selected)
          .then(() => {
            // Toast.fire({
            //   icon: "success",
            //   title: "deleted successfully",
            // });
            // this.getResults();
            Swal.fire("Deleted!", "Photo deleted successfully", "success");
            this.selectAll = false;
            this.selected = [];
            $(".sel").css('display' , 'none');
            // window.location.reload()
          })
          .catch(() => {
            Swal.fire({
              icon: "error",
              title: "Oops...",
              text: "Something went wrong!",
              footer: "<a href>Why do I have this issue?</a>",
            });
          });
        this.getResults();
      });
    },
    handleTasks(task) {
      // Do what you want with the selected objects:
      console.log(this.selectedTasks);
    },
    // getResults(page) {
    //   //  page_url = page_url || '../photo'
    //   if (typeof page == "undefined") {
    //     // i sayed to function if page undefined get currentHost of href
    //     // and if currentHost  == exampleURL(http://localhost:8000/edit#) page = 1 (the number of 1 is first page of data that get from backaend for paginate)
    //     var currentHost = window.location.href;
    //     var exampleURL = "http://localhost:8000/edit#";
    //     if (currentHost == exampleURL) {
    //       page = 1;
    //     } else {
    //       // for get number of page 1 set this function from js
    //       var numbers = currentHost.match(/\/edit#\/([0-9]+)/)[1];
    //       page = numbers;
    //     }
    //   }

    //   axios
    //     .get("../photo" + page)
    //     .then((response) => {
    //       this.books = response.data.data;
    //       this.makePagination(response.data);
    //       window.history.pushState("", "", "/edit#/" + page);
    //     })
    //     .catch((error) => {
    //       console.log(error);
    //       this.errored = true;
    //     })
    //     .finally(() => (this.loading = false));
    // },

    sign() {
      var redirect = this.$auth.redirect();
      var _this = this;
      var png = _this.$refs.signature.save();
      let currentObj = this;
      this.$http.post("auth/sign", { user_image: png }).then((result) => {
        _this.has_error = true;

        this.axios.get("http://localhost:8000/api/st").then((response) => {
          this.books = response.data;
          this.dismissCountDown = this.dismissSecs;
        });
      });
    },
    clear() {
      var _this = this;
      _this.$refs.signature.clear();
    },
    deleteBook(id) {
      this.axios.post(`../store_image/delete/${id}`).then((response) => {
        let i = this.books.map((item) => item.id).indexOf(id); // find index of your object
        this.books.splice(i, 1);
      });
    },
  },
  //  GET
};
</script>



<style >
.sel {
  display:none;
}

.inside > input[type=checkbox] {
  margin-left: 3em;
  border-radius: 25px;
  width: 20px;
}

/* .inside {
  position: relative;
  width: 120px;
  height: 40px;
  -webkit-appearance: initial;
  border-radius: 3px;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  outline: none;
  font-size: 14px;
  font-family: Trebuchet, Arial, sans-serif;
  font-weight: bold;
  cursor: pointer;
  border-radius: 25px;
  background: blueviolet;
  
}

.inside:after {
  position: absolute;
  top: 5%;
  display: block;
  line-height: 32px;
  width: 45%;
  height: 90%;
  background: blueviolet;
  box-sizing: border-box;
  text-align: center;
  transition: all 0.3s ease-in 0s;
  color: black;
  border: red 1px solid;
  border-radius: 25px;
}

.inside:after {
  left: 0%;
  content: "All";
  border-radius: 30px solid red;
}

.inside:checked:after {
  left: 53%;
  content: "NO";
  border-radius: 30px solid red;
  background: #9ad179;
  color: red;
}

.inside:before {

  display: inline;
  left: 10px;
  content: 'yes';
  color: red;
  font: 12px/26px Arial, sans-serif;
  font-weight: bold;
  text-transform: capitalize;
  z-index: 0;
} */



.checkbox > input[type=checkbox] {
  visibility: hidden;
}

.checkbox {
  position: relative;
  display: block;
  width: 80px;
  height: 26px;
  margin: 0 auto;
  background: #FFF;
  border: 1px solid #2E2E2E;
  border-radius: 2px;
  -webkit-border-radius: 2px;
  -moz-border-radius: 2px;
}

.checkbox:after {
  position: absolute;
  display: inline;
  right: 10px;
  content: 'no';
  color: #E53935;
  font: 12px/26px Arial, sans-serif;
  font-weight: bold;
  text-transform: capitalize;
  z-index: 0;
}

.checkbox:before {
  position: absolute;
  display: inline;
  left: 10px;
  content: 'yes';
  color: #43A047;
  font: 12px/26px Arial, sans-serif;
  font-weight: bold;
  text-transform: capitalize;
  z-index: 0;
}

.checkbox label {
  position: absolute;
  display: block;
  text-align: center;
  top: 2px;
  left: 3px;
  width: 34px;
  height: 20px;
  background: #2E2E2E;
  cursor: pointer;
  transition: all 0.5s linear;
  -webkit-transition: all 0.5s linear;
  -moz-transition: all 0.5s linear;
  border-radius: 2px;
  -webkit-border-radius: 2px;
  -moz-border-radius: 2px;
  z-index: 1;
  
}



.checkbox input[type=checkbox]:checked + label {
  left: 43px;
  background: chartreuse;
}


.checkbox input[type=checkbox]:checked + label::after {
  left: 43px;
  content: 'All';
  color: white;
  
}



/* bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb */
th , td {
  text-align: center;
}

.table th {
    vertical-align: baseline;
}
.table td {
    vertical-align: baseline;
}

</style>




