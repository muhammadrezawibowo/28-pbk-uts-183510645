<template>
  <div class="Register">
    <h1>Register Kegiatan</h1>
    <form class="container" @submit.prevent="tambahKegiatan">
      <div>
        <label for="npm">Nama</label>
        <input type="text" id="npm" v-model="kegiatan.npm" />
        <span class="error" v-show="!kegiatan.npm">Nama harus diisi</span>
      </div>
      <div>
        <label for="nama">Nama Kegiatan</label>
        <input type="text" id="nama" v-model="kegiatan.nama" />
        <span class="error" v-show="!kegiatan.nama">Nama Kegiatan harus diisi</span>
      </div>
      <div>
        <label for="kelas">Kelas</label>
        <input type="text" id="kelas" v-model="kegiatan.kelas" />
        <span class="error" v-show="!kegiatan.kelas">Kelas harus diisi</span>
      </div>
      <button type="submit">Tambahkan</button>
      <button type="button" @click="resetInput">Reset</button>
      <button type="button" @click="batalTambah" v-if="kegiatan.npm || kegiatan.nama || kegiatan.deskripsi || kegiatan.kelas">Batal</button>
    </form>
    <div>
      <label for="filter">Tampilkan Kegiatan Belum Selesai</label>
      <input type="checkbox" id="filter" v-model="filterDone" @change="filterKegiatan">
    </div>
    <div>
      <label for="search">Cari Kegiatan</label>
      <input type="text" id="search" v-model="searchText" @input="filterKegiatan" />
    </div>
    <ul class="list">
      <li v-for="(kegiatan, index) in kegiatanListFiltered" :key="index" :class="{ done: kegiatan.done, hidden: filterDone && kegiatan.done }">
        <h3>{{ kegiatan.nama }}</h3>
        <p>{{ kegiatan.npm }}</p>
        <p>{{ kegiatan.kelas }}</p>
        <button type="Delete" @click="hapusKegiatan(index)">Delete</button>
        <button @click="toggleDone(index)">
          {{ kegiatan.done ? 'Belum Selesai' : 'Sudah Selesai' }}
        </button>
      </li>
    </ul>
    <div class="footer1">
      <p>&copy; 2023 Muhamad Reza</p>
    </div>
  </div>
</template>


<script>
export default {

  data() {
    return {
      kegiatan: {
        nama: '',
        done: false
      },
      kegiatanList: [],
      filterDone: false,
      sortBy: 'default',
      searchText: ''
    }
  },

  computed: {
    kegiatanListFiltered() {
      if (this.searchText === '') {
        return this.kegiatanList
      } else {
        return this.kegiatanList.filter((kegiatan) => {
          const searchTextLower = this.searchText.toLowerCase()
          const namaLower= kegiatan.nama.toLowerCase()

          return namaLower.includes(searchTextLower)
        })
      }
    }
  },

  methods: {
    tambahKegiatan() {
      if (this.kegiatan.nama !== '') {
        this.kegiatanList.push({...this.kegiatan})
        this.resetInput()

        // menampilkan notifikasi
        if ('Notification' in window && Notification.permission === 'granted') {
          new Notification('Kegiatan berhasil ditambahkan')
        } else if ('Notification' in window && Notification.permission !== 'denied') {
          Notification.requestPermission().then((permission) => {
            if (permission === 'granted') {
              new Notification('Kegiatan berhasil ditambahkan')
            }
          })
        }
      }
    },


    resetInput() {
      this.kegiatan.nama = ''
    },

    hapusKegiatan(index) {
      if (confirm("Apakah Anda yakin ingin menghapus kegiatan ini?")) {
        this.kegiatanList.splice(index, 1);

        // menampilkan notifikasi
        if ('Notification' in window && Notification.permission === 'granted') {
          new Notification('Kegiatan berhasil dihapus')
        } else if ('Notification' in window && Notification.permission !== 'denied') {
          Notification.requestPermission().then((permission) => {
            if (permission === 'granted') {
              new Notification('Kegiatan berhasil dihapus')
            }
          })
        }
      }
    },


    toggleDone(index) {
      this.kegiatanList[index].done = !this.kegiatanList[index].done
    },

    batalTambah() {
      this.kegiatan.nama = ''
    },

    filterKegiatan() {
      // no need to do anything, computed property will take care of filtering
    },

  }
}
</script>


<style>
  .error {
    color: #3120E0;
    font-size: 14px;
    text-align:center;
    display: block;
    margin-bottom: 1rem;
  }
  form.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 40px;
  background-color: #f1f1f165;
  font-family: "Open Sans", sans-serif;
  color: #333;
  border-radius: 15px;
}


  .done {
    text-decoration: line-through;
  }

  .hidden {
    display: none;
  }

  .footer1 {
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #928f8f00;
    padding: 10px;
  }
  .footer {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    text-align: center;
    padding: 20px;
  }

  body {
    background-image: url('https://static.vecteezy.com/system/resources/previews/006/691/305/original/abstract-background-with-soft-gradient-color-and-dynamic-shadow-on-background-background-for-wallpaper-eps-10-free-vector.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    background-position:none;
  }

  @media screen and (max-width: 768px) {
    body {
      background-size: cover;
      background-position: none;
      
    }
  } 
  h1 {
    font-size: 3rem;
    text-align: center;
    margin-bottom: 2rem;
  }

  form {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100%;
  }

  form input[type="text"],
  form button[type="submit"] {
    width: 100%;
    max-width: 20rem;
    margin: 0.5rem 0;
  }

  form label {
    display: block;
    text-align: center;
    margin-bottom: 0.5rem;
  }

  input[type="text1"],
  textarea {
    padding: 0.5rem;
    font-size: 1.2rem;
    border-radius: 0.3rem;
    border: 0.5px solid #ccc;
  }

  button {
    padding: 0.8rem 1rem;
    font-size: 1.2rem;
    background-color: #539165;
    color: white;
    border: none;
    border-radius: 0.3rem;
    cursor: pointer;
    margin-right: 1rem;
  }


  button:hover {
    background-color: #FF6E31;
  }

  button.cancel {
    background-color: #ff9800;
  }

  button.cancel:hover {
    background-color: #f57c00;
  }

  button.delete:hover {
    background-color: #2757f5;
  }

  ul {
    list-style:none;
    margin: 0;
    padding: 0;
    widows: 1080px;;
  }

  li {
    border: 1px solid #000000;
    border-radius: 0.3rem;
    padding: 1rem;
    margin-bottom: 1rem;
    transition: all 0.3s ease-in-out;
    position: relative;
  }

  li:hover {
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    border-color: #4CAF50;
  }

  li button {
    position: absolute;
    top: 0;
    right: 0;
    padding: 0.4rem 2rem;
    font-size: 1rem;
    background-color:#00b7ff;
    color: white;
    border: none;
    border-top-right-radius: 0.3rem;
    cursor: pointer;
}
li button:hover {
background-color: #d32f2f;
}

li.done {
opacity: 0.5;
}

li.done button {
display: none;
}

li.hidden {
display: none;
}

li button:first-child {
    right: 6rem;
  }
  
  li button:last-child {
    right: 7rem;
  }
  
  li h3 {
    font-size: 1.5rem;
    margin-bottom: 0.5rem;
  }
  
  li p {
    font-size: 1.2rem;
    margin-bottom: 0.5rem;
  }

</style>

