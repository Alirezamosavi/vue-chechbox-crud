
for checkbox of delete we need a object (selected: []) + (selectAll: false) for varriable all checked


set input checkbox in part of th in table. by this checkbox we can checked all input checkbox to choose 

 <div class="checkbox">
                        <!-- by this input we can checked all input checkbox that there data-->
                      <input class="flipswitch" id="checkbox1" type="checkbox" v-model="selectAll" @click="select" />
                      <label for="checkbox1"></label>
                      
                      </div>
					  
					  
set other input checkbox for all data in td of table. by this input we can set or checked data to delete

<td v-if="!book.id == 0">
                    <!-- must set input checkbox here after v-for="book in books" -->
                    <!-- this is checkbox input to checked one by one of data to delete -->
                    <label class="form-checkbox inside">
                      <input type="checkbox" id="checkbox1" :value="book.id" v-model="selected"
                         />
                      <label for="checkbox1"></label>
                    </label>
                  </td>	


set a button for delete 

<button class="btn btn-danger" @click="Delete(selected)">
                      Delete
                    </button>

must a function set in method of vuejs for select data that will checked


put this in data of return
selected: [],
selectAll: false,

put in methodes

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

this is function to delete checkbox in controller

  public function delete_user($id)
    {
       $single_user_id = explode(',' , $id);          //  by this code we get each id that is request to delete
      

 

        foreach ($single_user_id as $id) {  // by this function we can get one by one id 
           $i = uploadimage::findOrFail($id);  // in here we get all id that request from user
           $i->delete();
           $currentPhoto = $i->photo;  // by this code i can find all photo that get from database so according that i can find photo in public/img folder

           $userPhoto = public_path('img/public/').$currentPhoto; 

           if(file_exists($userPhoto)) {  // if exist image or file in public/img then....

            @unlink($userPhoto);  // delete or move image from public/img
              
      }

        }
        
        return ['message' => 'deleted successfully'];

    }
	
	this is router
	
        Route::post('store_image/delete/{id}', 'App\Http\Controllers\PhotoController@delete_user');	// must add a id to find all data about that id like name or image in dataBASE 
	

every things is ok 
subscribe me to more tutorial of laravel and vuejs
thanks					

