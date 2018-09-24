<template>
  <div id="app">
    <div class="app__chat row">
      <form>
        <fieldset>
          <ul>
            <li v-for="(chatData, index) in chatInfo" :key="index">{{chatData.text}}</li>
          </ul>
          <input type="text" v-model="chatAnswears" placeholder="Type here..." id="nameField">
          <div class="float-right">
          </div>
          <input class="button-primary" @click.prevent="whatUserSaid(chatAnswears)" type="submit" value="Send">
        </fieldset>
      </form>

    </div>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        counter: 0,
        chatInfo: [{
          text: "What is your name my friend?"
        }],
        chatAnswears: [],
        chatQuestions: [{
            text: "What is your name my friend?"
          },
          {
            text: " that is beautiful. Where are you come from?"
          },
          {
            text: "How young you are?"
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
      responsiveVoice.speak(this.chatQuestions[0].text, "UK English Male");
    },
    methods: {
      whatUserSaid(data) {

        this.chatInfo.push({
          text: this.chatAnswears
        })

        switch (this.counter) {
          case 0:
            this.chatQuestions.shift(this.chatQuestions[0]);
            responsiveVoice.speak(this.chatAnswears);
            setTimeout(() => {
              this.chatInfo.push({
                text: `${this.chatAnswears}... ${this.chatQuestions[0].text}`
              });

              responsiveVoice.speak(this.chatQuestions[0].text, "UK English Male");
              this.chatAnswears = '';

              this.counter++

            }, 2000)

            break;

          default:
            this.chatQuestions.shift(this.chatQuestions[0]);
            responsiveVoice.speak(this.chatAnswears);
              this.chatInfo.push(this.chatQuestions[0].text);




            responsiveVoice.speak(this.chatQuestions[0].text, "UK English Male");
            this.chatAnswears = '';
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
    list-style-image: url('https://i0.wp.com/samsungprinter.app/wp-content/uploads/2018/05/icon-app.png?fit=25%2C25&ssl=1');
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
</style>
