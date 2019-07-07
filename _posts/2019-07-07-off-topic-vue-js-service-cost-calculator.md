---
layout: post
title: 'Off topic: Vue.js Service Cost Calculator'
summary: Learn how we built a service cost calculator for Courtneys website.
featured-img: "/assets/album/Screen Shot 2019-07-07 at 4.42.31 PM.png"
undefined: []

---
Hey everyone,

I thought I would write a very small post on when [Courtney](https://courtneymakesvideo.com/beta) asked [me](https://www.robertgabriel.ninja) to create her a service cost calculator for her [website](https://courtneymakesvideo.com/beta).

She wanted something where a potential customer could fill out a form with details on the video they are looking to get edited and be emailed with the price of how much the video would cost.

Well I thought, this was a great idea. So I found his example project on Code Pen by  [Amy Carrigan]() of a  [Vue.js Service Cost Calculator](https://codepen.io/amy_e_carrigan/pen/NpZvMR). It works great. I forked her code and changed it to Courtneys needs. That was more design, extra questions, photos of each service.

    <link rel='stylesheet' href='https://fonts.googleapis.com/css?family=Open+Sans:300,700|Rochester'>
    <link rel='stylesheet' href='https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css'>
    <link rel='stylesheet' href='https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.css'>
    
    
    
    <style>
        #main {
            min-height: 100vh !important;
            justify-content: center !important;
            align-items: center !important;
            font-family: "Arial" !important;
            color: #666666 !important;
        }
        .full_width {
            width: 100%;
        }
        .fa-check {
            -webkit-text-fill-color: #00701a;
            color: #00701a;
        }
        .container_ {
            width: 600px;
            margin-left: auto;
            margin-right: auto;
        }
        .heading {
            letter-spacing: .01rem !important;
            text-align: center !important;
            margin-bottom: 25px !important;
            font-size: 40px !important;
        }
        .align_center {
            text-align: center !important;
        }
        .heading_sub {
            margin-bottom: 30px;
        }
        .heading_services {
            font-size: 25px;
        }
        .services {
            margin-bottom: 1rem !important;
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            grid-template-rows: 1fr 1fr;
            grid-column-gap: 15px;
            grid-row-gap: 15px;
            list-style: none !important;
        }
        .services__item {
            cursor: pointer;
            justify-content: space-between !important;
            padding: 1rem !important;
            border-radius: .25rem !important;
            border: 1px solid rgba(0, 0, 0, .12);
            width: 150px;
            color: #006080 !important;
            background-color: #ebebeb !important;
        }
        .services__item_photo {
            width: 100%;
            background-color: blue;
            height: 150px;
            margin-bottom: 4px;
        }
        .services__item_photo>img,
        .services__item_photo img {
            width: 100%;
        }
        .services__item_text {
            text-align: center;
        }
        input[type="email"] {
            height: 25px;
            border-radius: 4px;
        }
        .styled {
            border: 0;
            margin-right: auto;
            margin-left: auto;
            margin-top: 20px;
            line-height: 2.5;
            width: 100%;
            padding: 0 20px;
            font-size: 1rem;
            text-align: center;
            color: #fff;
            text-shadow: 1px 1px 1px #000;
            border-radius: 10px;
            background-color: rgba(220, 0, 0, 1);
            background-image: linear-gradient(to top left, rgba(0, 0, 0, .2), rgba(0, 0, 0, .2) 30%, rgba(0, 0, 0, 0));
            box-shadow: inset 2px 2px 3px rgba(255, 255, 255, .6), inset -2px -2px 3px rgba(0, 0, 0, .6);
        }
        .styled:hover {
            background-color: rgba(255, 0, 0, 1);
        }
        .styled:active {
            box-shadow: inset -2px -2px 3px rgba(255, 255, 255, .6),
                inset 2px 2px 3px rgba(0, 0, 0, .6);
        }
        .extras {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr 1fr;
            grid-template-rows: 1fr;
            grid-column-gap: 15px;
            grid-row-gap: 15px;
            margin-top: 50px;
            list-style: none !important;
            margin-bottom: 30px;
        }
        .extras__item {
            cursor: pointer;
            justify-content: space-between !important;
            padding: 1rem !important;
            border-radius: .25rem !important;
            border: 1px solid rgba(0, 0, 0, .12);
            color: #006080 !important;
            background-color: #ebebeb !important;
        }
        .services__item.active {
            color: #ffffff !important;
            background-color: #ff5252 !important;
        }
        .total {
            justify-content: space-between !important;
            padding: 4.5rem !important;
            border-top: 1px solid #666666 !important;
        }
    </style>
    
    
    <div id="main" v-cloak>
        <div class="container_">
            <h1 class="heading">Instand Video Editing Quote</h1>
            <p class="heading_sub align_center">Fill out the options below to recieve an instant qoute emailed to you for an
                estimated cost
                for your project.</p>
    
            <h3 class="heading_services align_center">What Content Are We Creating Today?</h3>
            <p class="heading_sub align_center">Please select the category that best fits your project.</p>
    
            <ul class="services">
                <li class="services__item" v-for="service in services" v-on:click="toggleActive(service)">
                    <div class="services__item_photo"><img v-bind:src="service.url"></div>
                    <div class="services__item_text"><b>{{service.name}}</b></div>
                    <div class="align_center" v-if="service.active "><i class="fa fa-check" aria-hidden="true"></i></div>
                </li>
            </ul>
            <div class="total">
                <h3 class="heading_services align_center">How long will the video be?</h3>
                <p class="heading_sub align_center">{{length}} mins long</p>
                <input type="range" v-model="length" class="full_width" name="volume" min="2" max="180"
                    @change="changeRange">
            </div>
    
            <h3 class="heading_services align_center">What Extras Do you need?</h3>
            <p class="heading_sub align_center">Please select the category that best fits your project.</p>
    
            <ul class="extras">
                <li class="extras__item" v-for="extra in extras" v-on:click="toggleActive(extra)">
                    <div class="align_center">{{extra.name}}</div>
                    <div class="align_center" v-if="extra.active "><i class="fa fa-check" aria-hidden="true"></i></div>
                </li>
    
            </ul>
    
    
            <div class="total">
                <h3 class="heading_services align_center">Enter your email to get your quote today!</h3>
                <label for="email">Enter your email:</label>
                <input type="email" class="full_width wsite-form-input wsite-input wsite-input-width-370px" v-model="email"
                    required>
                <button class="full_width wsite-button" type="button" v-on:click="onSubmit">
                    Send me my quote
                </button>
            </div>
    
        </div>
    </div>
    <script src='https://cdnjs.cloudflare.com/ajax/libs/vue/2.1.10/vue.min.js'></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js">
    </script>
    
    <script>
        Vue.filter('currency', function (value) {
            return '$' + value.toFixed(2);
        });
        var app = new Vue({
            el: "#main",
            data: {
                api: 'https://courtney-app.herokuapp.com/email',
                length: 30,
                email: '',
                extras: [{
                        name: 'Animation/Motion Graphics',
                        price: 250,
                        active: true
                    },
                    {
                        name: 'Color Correction',
                        active: false
                    },
                    {
                        name: 'Sound Design/Mixing',
                        price: 250,
                        active: false
                    },
                    {
                        name: 'Visual FX/Greenscreen',
                        price: 250,
                        active: false
                    },
                ],
                services: [{
                        name: 'Youtube/Social Media',
                        price: 300,
                        url: 'https://www.weebly.com/editor/uploads/1/2/0/5/120502350/custom_themes/327876983747105144/files/DOCUMENTARY2.png',
                        active: true
                    },
                    {
                        name: 'Commerical',
                        price: 400,
                        url: 'https://www.weebly.com/editor/uploads/1/2/0/5/120502350/custom_themes/327876983747105144/files/DOCUMENTARY2.png',
                        active: false
                    },
                    {
                        name: 'Music Video',
                        price: 250,
                        url: 'https://www.weebly.com/editor/uploads/1/2/0/5/120502350/custom_themes/327876983747105144/files/DOCUMENTARY2.png',
                        active: false
                    },
                    {
                        name: 'Short Flim',
                        price: 220,
                        url: 'https://www.weebly.com/editor/uploads/1/2/0/5/120502350/custom_themes/327876983747105144/files/DOCUMENTARY2.png',
                        active: false
                    },
                    {
                        name: 'Documentary',
                        price: 220,
                        url: 'https://www.weebly.com/editor/uploads/1/2/0/5/120502350/custom_themes/327876983747105144/files/DOCUMENTARY2.png',
                        active: false
                    },
                    {
                        name: 'Wedding',
                        price: 220,
                        url: 'https://www.weebly.com/editor/uploads/1/2/0/5/120502350/custom_themes/327876983747105144/files/DOCUMENTARY2.png',
                        active: false
                    }
                ]
            },
            methods: {
                toggleActive: function (s) {
                    s.active = !s.active;
                },
                changeRange: function () {
                    console.log(this.length);
                },
                emailIsValid: function (email) {
                    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
                },
                 atLeastOne: function(values){
                   return values.some(v => v.active === true)
                } ,
                onSubmit: function () {
                    if (!this.email || !this.emailIsValid(this.email)) {
                        toastr.error('You need a valid email', 'Hey!')
                        return false;
                    }
                    if (this.atLeastOne(this.extras) === false){
                        toastr.error('You need to pick at least one extra feature', 'Hey!')
                        return false;
                    }
                    if (this.atLeastOne(this.services) === false){
                        toastr.error('You need to pick at least one service', 'Hey!')
                        return false;
                    }
                    toastr.info('Sending your qoute now', 'Hey!')
                    axios.post(this.api, {
                            length: this.length,
                            email: this.email,
                            services: this.services,
                            extras: this.extras,
                        })
                        .then(function (response) {
                            toastr.success('I got your request and we will back to you in under 24 hours.',
                                'Courtney says')
                            console.log(response);
                        })
                        .catch(function (error) {
                            toastr.error('Something happened, we are on it', 'Inconceivable!')
                            console.log(error);
                        });
                },
                total: function () {
                    var total = 0;
                    this.services.forEach(function (s) {
                        if (s.active) {
                            total += s.price;
                        }
                    });
                    return total;
                }
            }
        });
    </script>
    
    
    
    
    <script
        src="https://static.codepen.io/assets/editor/live/css_reload-5619dc0905a68b2e6298901de54f73cefe4e079f65a75406858d92924b4938bf.js">
    

I then added the functions to allow the forum to be submitted to a remote [API](https://courtney-app.herokuapp.com/), which is being hosted on [Heroku](https://courtney-app.herokuapp.com/). 

    const app = require('express')();
    const nodemailer = require('nodemailer');
    const bodyParser = require('body-parser');
    const cors = require('cors');
    app.use(cors());
    app.use(bodyParser.json()); // to support JSON-encoded bodies
    app.use(bodyParser.urlencoded({ // to support URL-encoded bodies
      extended: true
    }));
    
    
    // Default routing file
    /**
     * @param  {} '/'
     * @param  {} (req
     * @param  {} res
     */
    app.get('/', (req, res) => {
      res.setHeader('Content-Type', 'application/json');
    
    
      res.send(
        JSON.stringify({
          message: 'I love you <3, sorry this was late.'
        })
      );
    });
    
    /**
     * @param  {} '/donate'
     * @param  {} (req
     * @param  {} res
     */
    app.post('/email', (req, res) => {
    
    
      try {
        const videoLength = req.body.length;
        const email = req.body.email;
        const services = req.body.services;
        const extras = req.body.extras;
        var transporter = nodemailer.createTransport({
          service: 'gmail',
          auth: {
            user: 'gtirob@gmail.com',
            pass: ''
          }
        });
    
        let bodyText = `Hey!! <br> A user with email <b>${email}</b> used your contact fourm looking for a price with the following information.`;
        bodyText += '<br><br><b>Length</b><br><br>';
        bodyText += `<b>About ${videoLength} mins </b><br><br>`;
        bodyText += '<b>Category</b><br><br>';
        bodyText += '<ul>';
        services.forEach(function (s) {
          if (s.active) {
            bodyText += `<li>${s.name} at $ ${s.price}</li>`;
          }
        });
        bodyText += '</ul>';
    
        bodyText += '<b>Extras needed</b><br><br>';
        bodyText += '<ul>';
        extras.forEach(function (e) {
          if (e.active) {
            bodyText += `<li>${e.name} at $ ${e.price}</li>`;
          }
        });
        bodyText += '</ul>';
    
        var total = 0;
    
        services.forEach(function (s) {
          if (s.active) {
            total += s.price;
          }
        });
    
        extras.forEach(function (e) {
          if (e.active) {
            total += e.price;
          }
        });
    
    
        bodyText += `<br><br> Your total price is : ${total}`;
    
        bodyText += `<br><br> Thank you, <br><br><br> Courntey Hood`
        var mailOptions = {
          from: 'gtirob@gmail.com',
          bcc: 'courtneymakesvideo@gmail.com',
          to: email,
          subject: 'Heres your qoute from Courtney Hood',
          html: bodyText
        };
    
        transporter.sendMail(mailOptions, function (error, info) {
          if (error) {
            console.log(error);
          } else {
            console.log('Email sent: ' + info.response);
            res.end();
          }
        });
    
      } catch (e) {
        res.status(400).send('Invalid JSON string');
        res.end();
      }
      res.end();
    });
    
    /**
     * @param  {} 'port'
     * @param  {} process.env.PORT||5000
     */
    app.set('port', process.env.PORT || 5000);
    
    // Spin up the server
    /**
     * @param  {} app.get('port'
     * @param  {} function(
     */
    app.listen(app.get('port'), () => {
      console.log('running on port', app.get('port'));
    });

The user submits the forum to the server which looks at the selected options and calculates the price. Sending it to both the price and to Courtney's email. So they can follow up after.

It is a really fun project and I hope this helps any digital nomad looking to create a [Vue.js Service Cost Calculator](https://codepen.io/amy_e_carrigan/pen/NpZvMR).