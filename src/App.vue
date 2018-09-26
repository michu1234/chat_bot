<template>
  <div id="app">
    <img class="app__bot" :src="src" />
    <div class="app__chat row">
      <form>
        <fieldset>
          <ul>
            <li v-for="(chatData, index) in chatInfo" :key="index">{{chatData.text}}</li>
          </ul>
          <input type="text" v-model="chatAnswears" ref="input" placeholder="Type here..." id="nameField">
          <div class="float-right">
          </div>
          <input class="button-primary" ref="submit" @click.prevent="whatUserSaid(chatAnswears)" type="submit" value="Send">
        </fieldset>
      </form>

    </div>

  </div>
</template>

<script>
  import {
    TimelineLite
  } from 'gsap'
  export default {
    data() {
      return {
        src: './static/mowi.gif',
        counter: 0,
        chatInfo: [{
          text: "What is your name my friend?"
        }],
        chatAnswears: [],
        questionsNumber: '',
        chatQuestions: [{
            text: "What is your name my friend?"
          },
          {
            text: " that is beautiful. Where are you come from"
          },
          {
            text: "! 😲 What a amazing place to live! How young you are?"
          },
          {
            text: "What is your e-mail adress?"
          },
          {
            text: "Thank you for a little chat 😏"
          }
        ]
      }
    },
    mounted() {
      this.botSpeaks();
    },
    created() {
      this.questionsNumber = this.chatQuestions.length;
    },
    methods: {
      botSpeaks() {
        this.src = '/img/mowi.394f1d09.gif';
        responsiveVoice.speak(this.chatQuestions[0].text, "UK English Male");
        setTimeout(() => {
          this.src = '/img/milczy.d51d6997.gif';
        }, 3000)
      },
      userSpeaks() {
        responsiveVoice.speak(this.chatAnswears);
      },
      nextQuesition() {
        this.chatQuestions.shift(this.chatQuestions[0]);
        setTimeout(() => {
          this.chatInfo.push({
            text: this.chatQuestions[0].text
          });
        }, 2000);
      },
      whatUserSaid(data) {
        const {
          submit,
          inputborder
        } = this.$refs;
        const timeline = new TimelineLite();
        this.chatInfo.push({
          text: this.chatAnswears
        })

        switch (this.counter) {
          case 0:
            this.userSpeaks();



            this.chatQuestions.shift(this.chatQuestions[0]);
            setTimeout(() => {
              this.chatInfo.push({
                text: `${this.chatAnswears}... ${this.chatQuestions[0].text} ${this.chatAnswears}?`
              });
              this.src = '/img/mowi.394f1d09.gif';
              responsiveVoice.speak(`${this.chatAnswears}... ${this.chatQuestions[0].text}${this.chatAnswears}?`,
                "UK English Male");
              setTimeout(() => {
                this.src = '/img/milczy.d51d6997.gif';
              }, 4000)
              this.chatAnswears = '';
              this.counter++;
            }, 2000)



            break;

          case 1:
            this.userSpeaks();



            this.chatQuestions.shift(this.chatQuestions[0]);
            setTimeout(() => {
              this.chatInfo.push({
                text: `${this.chatAnswears}${this.chatQuestions[0].text}`
              });
              this.src = '/img/mowi.394f1d09.gif';
              responsiveVoice.speak(`${this.chatAnswears}... ${this.chatQuestions[0].text}`, "UK English Male");
              setTimeout(() => {
                this.src = '/img/milczy.d51d6997.gif';
              }, 4500)
              this.chatAnswears = '';
              this.counter++;
            }, 2000)




            break;
          case 4:
            this.userSpeaks();
            this.chatQuestions.shift(this.chatQuestions[0]);
            setTimeout(() => {
              this.chatInfo.push({
                text: "Thank you!"
              });
            }, 2000)



            this.counter++
            this.chatAnswears = '';
            break;
          case 5:
            this.$refs.input.disabled = true;
            this.$refs.submit.disabled = true;
            this.chatAnswears = '';
            this.$refs.input.placeholder = "See you soon! 😉 👋👋";
            break;
          default:
            this.userSpeaks();
            this.chatAnswears = '';
            this.nextQuesition();
            this.botSpeaks();
            this.counter++
            break;
        }
      }
    }
  }
</script>

<style lang="scss">
  .app__chat {
    position: absolute;
    bottom: 0;
    left: 100px;
  }

  ul li {
    list-style: none;
    font-weight: bold;
    list-style-image: url('https://image.ibb.co/grBPU9/D_ejson_Html.jpg');
    border-bottom: 1px dashed lightgray;
    animation: animate 1s ease;
    transition: all 1s;
  }

  li:nth-child(even) {
    color: #9b4dca;
    transform: translate(100px);
    max-width: 200px;
    list-style-image: url('http://maxpacerelo.com/maxpace//wp-content/uploads/2015/04/testimonial-icon-1.png');
    animation: animateEven 1s ease;
  }


  @keyframes animate {
    from {
      opacity: 0;
      transform: translateY(100px);
    }

    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  ;

  @keyframes animateEven {
    from {
      opacity: 0;
      transform: translateY(100px);
    }

    to {
      opacity: 1;
      transform: translate(100px, 0);
    }
  }

  .app_fadingBox {
    width: 100%;
    height: 150px;
    background: white;
    box-shadow: 0 0 90px white,
      0 0 90px white,
      0 0 90px white,
      0 0 90px white;
    opacity: .6;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 1;
  }

  .app__bot {
    position: fixed;
    right: 0;
    bottom: 0;
    transform: translateX(200px);
  }
</style>
