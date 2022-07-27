<script>

import axios from "axios";

export default {
  props: {
    item: { type: Object },
  },

  
  methods: {},
  created: function () {
    var editobjmain = this;

    var dest = editobjmain.$options.apiHost + '/' + editobjmain.$options.name;
    
    var apiconfig = null;
    
    if (typeof editobjmain.$options.apiConfig != "undefined") {
      apiconfig = editobjmain.$options.apiConfig;
    }


      editobjmain.item.save = async function (e) {
      if (editobjmain.item.id > 0) {
        await axios.patch(dest+"/"+editobjmain.item.id, editobjmain.item,apiconfig).then(response=>{
           e.onSaved(this,response);
        });
      } else {
        await axios.post(dest, editobjmain.item,apiconfig).then(response=>{
            
            editobjmain.item.id = response.data[editobjmain.$options.name].id;
            e.onSaved(this,response);
            
        });

        
      }
      
    };

    editobjmain.item.delete = async function (e) {
    await axios.delete(dest+"/"+editobjmain.item.id, editobjmain.item,apiconfig).then(response=>{
          
           e.onDeleted(this,response);
           
        });
      
    };

    
  },
};
</script>
