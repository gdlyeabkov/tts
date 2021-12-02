<template>
  <div>
    <Header />
    <div class="main">
      <div class="ttsForm">
        <div class="headerBtns">
          <div>
            <button class="headerBtn btn btn-danger headerBtn" @click="text = ''">
              Очистить текст
            </button>
            <button class="headerBtn btn btn-danger headerBtn" @click="this.isPlay = true; convertTextToSpeech()">
              Переиграть
            </button>
          </div>
          <select v-model="language" class="ttsFormItem ttsFormSelectorHeader form-select btn-danger w-50" required aria-label="select example">
            <option :value="'rus'">Русский</option>
            <option :value="'eng'">English</option>
          </select>
            
        </div>
        <div class="ttsFormItem playBtnContainer">
          <button class="playBtn material-icons btn btn-danger" @click="convertTextToSpeech">
            {{
              !isPlay ?
                'play_arrow'
              :
                'pause'
            }}
          </button>
        </div>
        <textarea placeholder="*Введите здесь текст." v-model="text" class="form-control ttsFormItem ttsFormText"></textarea>
        <button class="ttsFormItem btn btn-danger w-25" @click="uploadFromTextFile">
          Загрузить файл
        </button>
        <input type="file" v-show="false" ref="fileUploader" />
        <select v-model="speed" class="ttsFormItem ttsFormSelectorHeader form-select btn-danger w-75" required aria-label="select example">
          <option :value="0.1">0.1</option>
          <option :value="0.2">0.2</option>
          <option :value="0.3">0.3</option>
          <option :value="0.4">0.4</option>
          <option :value="0.5">0.5</option>
          <option :::value="0.6">0.6</option>
          <option :value="0.7">0.7</option>
          <option :value="0.8">0.8</option>
          <option :value="0.9">0.9</option>
          <option :value="1.0">Скорость по умолчанию</option>
          <option :value="1.0">1.0</option>
          <option :value="1.1">1.1</option>
          <option :value="1.2">1.2</option>
          <option :value="1.3">1.3</option>
          <option ::value="1.4">1.4</option>
          <option :value="1.5">1.5</option>
          <option ::value="1.6">1.6</option>
          <option ::value="1.7">1.7</option>
          <option :value="1.8">1.8</option>
          <option :value="1.9">1.9</option>
          <option :value="2.0">2.0</option>
        </select>
        <select v-model="pitch" class="ttsFormItem ttsFormSelectorHeader form-select btn-danger w-75" required aria-label="select example">
          <option :value="0.1">0.1</option>
          <option :value="0.2">0.2</option>
          <option :value="0.3">0.3</option>
          <option :value="0.4">0.4</option>
          <option :value="0.5">0.5</option>
          <option :::value="0.6">0.6</option>
          <option :value="0.7">0.7</option>
          <option :value="0.8">0.8</option>
          <option :value="0.9">0.9</option>
          <option :value="1.0">Тон по умолчанию</option>
          <option :value="1.0">1.0</option>
          <option :value="1.1">1.1</option>
          <option :value="1.2">1.2</option>
          <option :value="1.3">1.3</option>
          <option ::value="1.4">1.4</option>
          <option :value="1.5">1.5</option>
          <option ::value="1.6">1.6</option>
          <option ::value="1.7">1.7</option>
          <option :value="1.8">1.8</option>
          <option :value="1.9">1.9</option>
          <option :value="2.0">2.0</option>
        </select>
        <!-- <button class="ttsFormSelector ttsFormItem btn btn-danger w-75" @click="toggleSpeed">
          <span class="ttsFormSelectorHeader">
            {{
              speed === 1.0 ?
                'Скорость по умолчанию'
              :
                speed
            }}
          </span>
          <span class="material-icons ttsFormSelectorHeader">
            expand_more
          </span>
        </button>
        <button class="ttsFormSelector ttsFormItem btn btn-danger w-75" @click="togglePitch">
          <span>
            {{
              pitch === 1.0 ?
                'Тон по умолчанию'
              :
                pitch
            }}
          </span>
          <span class="material-icons">
            expand_more
          </span>
        </button> -->
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>

import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'

import speak from 'offline-tts'

export default {
  name: 'Home',
  data() {
    return {
      text: '',
      voice: 0,
      speed: 1.0,
      volume: 1.0,
      pitch: 1.0,
      filesList: [],
      isPlay: false,
      language: 'rus'
    }
  },
  mounted() {
    setInterval(() => {
      if (!window.speechSynthesis.speaking) {
        this.isPlay = false
      }
    }, 1000) 
  },
  methods: {
    FileListItems(files){
      var b = new ClipboardEvent("").clipboardData || new DataTransfer()
      for (var i = 0, len = files.length; i<len; i++){
        b.items.add(files[i])
      }
      return b.files
    },
    uploadFromTextFile() {
      window.showOpenFilePicker({     
        types: [
          {
            description: 'Поддерживаемые файлы',
            accept: {
              'text/plain': ['.txt'],
            },
          },
        ],
        excludeAcceptAllOption: true,
        multiple: false,
      }).then(async files => {
        this.filesList = []
        for(let file of files){
          this.filesList.push(await file.getFile())
        }
        this.$refs.fileUploader.files = new this.FileListItems(
          this.filesList
        )
        let reader = new FileReader()
        reader.addEventListener('load', (event) => {
          this.text = event.target.result
        })
        reader.readAsText(this.filesList[0])
        this.text = 'Загрузка...'
      }).catch(e => console.log('Не могу открыть диалог: ', e))
    },
    toggleSpeed() {
      this.speed = this.speed === 1.0 ?
        1.1
      : this.speed === 1.1 ?
        1.2
      : this.speed === 1.2 ?
        1.3
      : this.speed === 1.3 ?
        1.4
      : this.speed === 1.4 ?
        1.5
      : this.speed === 1.5 ?
        1.6
      : this.speed === 1.6 ?
        1.7
      : this.speed === 1.7 ?
        1.8
      : this.speed === 1.8 ?
        1.9
      : this.speed === 1.9 ?
        2.0
      : this.speed === 2.0 ?
        0.1
      : this.speed === 0.1 ?
        0.2
      : this.speed === 0.2 ?
        0.3
      : this.speed === 0.3 ?
        0.4
      : this.speed === 0.4 ?
        0.5
      : this.speed === 0.5 ?
        0.6
      : this.speed === 0.6 ?
        0.7
      : this.speed === 0.7 ?
        0.8
      : this.speed === 0.8 ?
        0.9
      : this.speed === 0.9 ?
        1.0
      :
        1.0
    },
    togglePitch() {
      this.pitch = this.pitch === 1.0 ?
        1.1
      : this.pitch === 1.1 ?
        1.2
      : this.pitch === 1.2 ?
        1.3
      : this.pitch === 1.3 ?
        1.4
      : this.pitch === 1.4 ?
        1.5
      : this.pitch === 1.5 ?
        1.6
      : this.pitch === 1.6 ?
        1.7
      : this.pitch === 1.7 ?
        1.8
      : this.pitch === 1.8 ?
        1.9
      : this.pitch === 1.9 ?
        2.0
      : this.pitch === 2.0 ?
        0.1
      : this.pitch === 0.1 ?
        0.2
      : this.pitch === 0.2 ?
        0.3
      : this.pitch === 0.3 ?
        0.4
      : this.pitch === 0.4 ?
        0.5
      : this.pitch === 0.5 ?
        0.6
      : this.pitch === 0.6 ?
        0.7
      : this.pitch === 0.7 ?
        0.8
      : this.pitch === 0.8 ?
        0.9
      : this.pitch === 0.9 ?
        1.0
      :
        1.0
    },
    convertTextToSpeech() {
      this.isPlay = !this.isPlay
      if (this.isPlay) {
        if (this.text.length >= 1) {
          speak(this.text, this.voice, this.speed, this.volume, this.pitch)
        } else if (this.text.length <= 0) {
          alert('Вы не ввели текст')
          this.isPlay = !this.isPlay
        }
        
      } else if (!this.isPlay) {
        speak('', this.voice, this.speed, 0, this.pitch)
      }
    }
  },
  components: {
    Header,
    Footer    
  }
}
</script>

<style scoped>
  
  .main {
    background-color: rgb(230, 230, 230);
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
  }

  .ttsForm {
    box-sizing: border-box;
    padding: 35px;
    width: 65%;
    background-color: rgb(255, 255, 255);
    height: 750px;
    display: flex;
    flex-direction: column;
  }

  .playBtnContainer {
    display: flex;
    justify-content: center;
  }

  .playBtn {
    font-size: 48px;
  }

  .ttsFormItem {
    margin: 5px 0px;
  }

  .ttsFormText {
    box-sizing: border-box;
    padding: 25px;
    height: 350px;
    font-weight: 500;
  }

  .ttsFormSelector {
    display: flex;
    justify-content: space-between;
  }

  .ttsFormSelectorHeader {
    color: rgb(255, 255, 255) !important;
  }

  .headerBtns {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 15px 0px;
  }

  .headerBtn {
    margin: 0px 5px;
  }

</style>