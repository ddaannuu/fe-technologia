<template>
  <div style="padding:30px">
    <h2>Test Upload Supabase</h2>

    <input type="file" @change="uploadFile">

    <br><br>

    <img
      v-if="imageUrl"
      :src="imageUrl"
      width="300"
    >

    <p>{{ imageUrl }}</p>
  </div>
</template>

<script>
import { supabase } from '@/lib/supabase'

export default {

  data() {
    return {
      imageUrl: ""
    }
  },

  methods: {

    async uploadFile(event) {

      const file = event.target.files[0]

      if (!file) return

      const fileName = Date.now() + "-" + file.name

      const { error } = await supabase.storage
        .from("products")
        .upload(fileName, file)

      if (error) {

        alert(error.message)

        return

      }

      const { data } = supabase.storage
        .from("products")
        .getPublicUrl(fileName)

      this.imageUrl = data.publicUrl

      alert("Upload berhasil")

    }

  }

}
</script>