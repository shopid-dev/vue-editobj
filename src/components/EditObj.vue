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


      editobjmain.item.save = async function (e) {
      if (editobjmain.item.id > 0) {
        await axios.patch(dest+"/"+editobjmain.item.id, editobjmain.item).then(response=>{
            console.log(response.data[editobjmain.$options.name].id)
        });
      } else {
        await axios.post(dest, editobjmain.item).then(response=>{
            console.log(response.data[editobjmain.$options.name].id)
            editobjmain.item.id = response.data[editobjmain.$options.name].id;
        });

        
      }
      e.onSaved(this);
    };

    editobjmain.item.delete = function (e) {
    axios.delete(dest+"/"+editobjmain.item.id, editobjmain.item);
      e.onDeleted(this);
    };

    
  },
};
</script>