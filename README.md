
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


every things is ok 
subscribe me to more tutorial of laravel and vuejs
thanks					

