<template>
  <div class="home">
 <div>
    <v-toolbar flat color="white">
      <v-toolbar-title>ตรวจสอบ Meetup</v-toolbar-title>
      <v-divider
        class="mx-2"
        inset
        vertical
      ></v-divider>
      <v-spacer></v-spacer>
      <v-dialog v-model="dialog" max-width="500px">
        <v-btn slot="activator" color="primary" dark class="mb-2">Add Meetup</v-btn>
        <v-card>
          <v-card-title>
            <span class="headline">{{ formTitle }}</span>
          </v-card-title>

          <v-card-text>
            <v-container grid-list-md>
              <v-layout wrap>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="editedItem.title" label="ชื่องาน"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="editedItem.date" label="วันที่"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="editedItem.time" label="เวลา"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="editedItem.location" label="สถานที่"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field v-model="editedItem.Abstract" label="รายละเอียด"></v-text-field>
                </v-flex>
              </v-layout>
            </v-container>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" flat @click="close">Cancel</v-btn>
            <v-btn color="blue darken-1" flat @click="save">Save</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-toolbar>
    <v-data-table
      :headers="headers"
      :items="desserts"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td>{{ props.item.title }}</td>
        <td class="text-xs-right">{{ props.item.date }}</td>
        <td class="text-xs-right">{{ props.item.time }}</td>
        <td class="text-xs-right">{{ props.item.location }}</td>
        <td class="text-xs-right">{{ props.item.Abstract }}</td>
        <td class="justify-center layout px-0">
          <v-icon
            small
            class="mr-2"
            @click="editItem(props.item)"
          >
            edit
          </v-icon>
          <v-icon
            small
            class="mr-2"
            @click="deleteItem(props.item)"
          >
            delete
          </v-icon>
          <v-icon
            small
            @click="verify(props.item)"
          >
            how_to_reg
          </v-icon>
        </td>
      </template>
      <template slot="no-data">
        <v-btn color="primary" @click="initialize">Reset</v-btn>
      </template>
    </v-data-table>
  </div>
 </div>
</template>

<script>

export default {
  data: () => ({
    dialog: false,
    headers: [
      {
        text: 'ชื่องาน',
        align: 'left',
        sortable: false,
        value: 'name'
      },
      { text: 'วันที่', value: 'calories' },
      { text: 'เวลา', value: 'fat' },
      { text: 'สถานที่', value: 'carbs' },
      { text: 'รายละเอียด', value: 'protein' },
      { text: 'Actions', value: 'name', sortable: false }
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      title: '',
      date: '',
      time: '',
      location: '',
      Abstract: ''
    },
    defaultItem: {
      title: '',
      date: '',
      time: '',
      location: '',
      Abstract: ''
    }
  }),
  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
    }
  },
  watch: {
    dialog (val) {
      val || this.close()
    }
  },
  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      this.desserts = [
        {
          title: 'ชื่องานที่จะจัด meetup1',
          date: '01/02/2562',
          time: '09.00 น. - 12.00 น.',
          location: 'ศูนย์คอมพิวเตอร์ สจล.ชั้น 1',
          Abstract: 'Lorem ipsum dolor sit ame'

        },
        {
          title: 'ชื่องานที่จะจัด meetup2',
          date: '01/02/2562',
          time: '09.00 น. - 12.00 น.',
          location: 'ศูนย์คอมพิวเตอร์ สจล.ชั้น 1',
          Abstract: 'Lorem ipsum dolor sit ame'
        }
      ]
    },
    editItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      const index = this.desserts.indexOf(item)
      confirm('Are you sure you want to delete this item?') && this.desserts.splice(index, 1)
    },

    verify (item) {
      confirm('Are you sure you want to verify this item?')
    },
    close () {
      this.dialog = false
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },
    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    }
  }
}
</script>
