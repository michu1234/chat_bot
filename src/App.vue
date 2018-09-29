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
          <input class="button-primary" ref="submit" @click.prevent="chat(chatAnswears)" type="submit" value="Send">
        </fieldset>
      </form>

    </div>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        src: '',
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
      let botSpeech = this.chatQuestions[0].text;
      this.botSpeaks(botSpeech, 3000);
      this.counter++;
    },
    created() {
      this.questionsNumber = this.chatQuestions.length;
    },
    methods: {
      botSpeaks(data, deleay, delay2 = 1500) {
        this.src = './static/milczy.gif';
        setTimeout(() => this.src = './static/mowi.gif', delay2)
        responsiveVoice.speak(data, "UK English Male");
        setTimeout(() => {
          this.src = './static/milczy.gif';
        }, deleay)
      },
      userSpeaks() {
        responsiveVoice.speak(this.chatAnswears);
      },
      nextQuesition(data, callback) {
        this.chatQuestions.shift(this.chatQuestions[0]);
        setTimeout(() => {
          this.chatInfo.push({
            text: data
          });
          callback();
        }, 2000);
      },
      pushNewUserAnswear(data) {
        this.chatInfo.push({
          text: data
        })
      },
      chat(data) {
        this.pushNewUserAnswear(data);
        this.userSpeaks();

        switch (this.counter) {
          case 1:
            let botSpeech = `${data} ${this.chatQuestions[1].text} ${data}`;
            this.nextQuesition(botSpeech, this.botSpeaks(botSpeech, 5000));
            this.chatAnswears = '';
            this.counter++;
            break;
          case 2:
            let botSpeech2 = `${data} ${this.chatQuestions[1].text}`;
            this.nextQuesition(botSpeech2, this.botSpeaks(botSpeech2, 6000));
            this.chatAnswears = '';
            this.counter++;
            break;
          case 4:
            let botSpeech4 = `${this.chatQuestions[1].text}`;
            this.nextQuesition(botSpeech4, this.botSpeaks(botSpeech4, 5000, 3000));
            this.chatAnswears = '';
            this.counter++;
          case 5:
            this.$refs.input.disabled = true;
            this.$refs.input.placeholder = ":-)";
            this.$refs.submit.disabled = true;
            this.chatAnswears = '';
            this.$refs.input.placeholder = "See you soon! 😉 👋👋";
            break;
          default:
            let botSpeech3 = `${this.chatQuestions[1].text}`;
            this.nextQuesition(botSpeech3, this.botSpeaks(botSpeech3, 4000));
            this.chatAnswears = '';
            this.counter++;
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

  .app__bot {
    position: fixed;
    right: 0;
    bottom: 0;
    transform: translateX(200px);
  }
</style>
