# 纯 CSS 绘制信封

```html
<!DOCTYPE html>
<html>
    <head>
        <link rel="preconnect" href="<https://fonts.gstatic.com>">
        <link href="<https://fonts.googleapis.com/css2?family=Bodoni+Moda&display=swap>" rel="stylesheet">
        <link href="<https://fonts.googleapis.com/css2?family=Bodoni+Moda&family=Lobster&display=swap>" rel="stylesheet">
        <style>
            .envelope {
                background: repeating-linear-gradient(
                    135deg,
                    #E7003E 0px,
                    #E7003E 40px,
                    white 40px,
                    white 80px,
                    #162EAE 80px,
                    #162EAE 120px,
                    white 120px,
                    white 160px
                );
                height: 500px;
                width: 800px;
                margin: auto;
                padding: 2% 0 2% 0;
                border-radius: 3%;
                box-shadow: 0 10px 30px rgba(0,0,0,0.19), 0 15px 30px rgba(0,0,0,0.23);
;
            }
            .whiteboard {
                background-color: white;
                height: 99%;
                width: 95%;
                margin: auto;
                border-radius: 3%;
                border-color: gray;
            }
            .caption {
                position: relative;
                top: 6%;
                font-size: 50px;
                font-family: Lobster, serif;
                text-align: center;
                display: block;
            }
            .input_form {
                margin-top: 4em;
                margin-bottom: 5em;
                margin-left: auto;
                margin-right: auto;
                text-align: center;
            }
            input {
                width: 50%;
                border-width: 0px;
                outline-style: none;
                font-size: 1.2em;
                border-bottom-width: 2px;
                padding: 6px;
                border-bottom-color: gray;
                font-weight: 300;
                font-family:'Bodoni Moda', serif;
            }
            input:hover {
                border-bottom-color: #6477e7;
            }
            input::-webkit-input-placeholder {
                color: rgb(187, 187, 187);
            }
            input:active {
                border-bottom-color: #354edf;
            }
            .input_place {
                display: block;
                margin: 20px 0 20px 0;
            }
            /* #say {
                height: 60px;
            } */
            .confirm {
                text-align: center;
            }
            .confirm_button {
                font-size: 1.3em;
                font-family: Lobster, 'Bodoni Moda', serif;
                color: white;
                background-color: #6477E7;
                padding: 16px 50px 16px 50px;
                border-radius: 30px;
                text-decoration: none;
            }
            .confirm_button:hover {
                background-color: #354edf;
                box-shadow: 0 16px 30px 6px rgba(0,0,0,0.19);
                animation-duration: 0.5s;
            }
        </style>
    </head>
    <body>
        <div class="envelope">
            <div class="whiteboard">
                <div class="caption">
                    CONTACT ME
                </div>
                <div class="input_form">
                    <label for="name" class="input_place">
                        <input type="text" placeholder="Your name">
                    </label>
                    <label for="email" class="input_place">
                        <input type="text" placeholder="Your Email">
                    </label>
                    <label for="content" class="input_place">
                        <input id="say" type="text" placeholder="Say something">
                    </label>
                </div>
                <div class="confirm">
                    <a class="confirm_button" href="#">Send</a>
                </div>
            </div>
        </div>
    </body>
</html>
```