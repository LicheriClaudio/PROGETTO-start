
<<<<<<< HEAD
[Shown link](file:///C:/Users/claud/OneDrive/Desktop/prog%20settimanali/PROGETTO-start/index.html)
=======
[CLICCA QUI](http://127.0.0.1:5500/index.html)


[HTML]

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css" integrity="sha512-iBBXm8fW90+nuLcSKlbmrPcLa0OT92xO1BIsZ+ywDWZCvqsWgccV3gFoRBv0z+8dLJgyAHIhR35VZc2oM/gI1w==" crossorigin="anonymous" />

    <link href="https://fonts.googleapis.com/css2?family=Lexend&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="css/style.css">
    <title>Memory Game</title>
</head>

<body>
    <!-- <div id="titolo">
        <h1> Memory</h1>
    </div>
 -->

    <div class="container icon-grid" id="griglia">
    </div>
    <div class="container text-center" id="reset">
        <input type="button" id="button" class="button" onclick=startGame() value="Ricomincia">
        <div class="timer">Tempo: 0 min 0 sec</div>
    </div>

    <div id="modal">
        <div class="content">
            <h2>Congratulazioni! Hai risolto il gioco in: </h2>
            <p><span id="tempoTrascorso"> </span> </p>
            <p><input type="button" class="button" onclick=playAgain() value="Gioca Ancora!"></p>
        </div>
    </div>

    <script src="js/memory.js">
    </script>
</body>

</html>

[JAVASCRIPT]

var arrayAnimali = ['🐱', '🦉', '🐾', '🦁', '🦋', '🐛', '🐝', '🐬', '🦊', '🐨', '🐰', '🐯', '🐱', '🦉', '🐾', '🦁', '🦋', '🐛', '🐝', '🐬', '🦊', '🐨', '🐯', '🐰'];
//libreria per icone
//https://html-css-js.com/html/character-codes/


var arrayComparison = [];

document.body.onload = startGame();

// mi serviranno alcune variabili 1. interval 2. una agganciata alla classe find 
// 3. una agganciata al'id modal 4. una agganciata alla classe timer


//una funzione che serve a mescolare in modo random gli elementi dell'array che viene passato 

function shuffle(a) {
    var currentIndex = a.length;
    var temporaryValue, randomIndex;

    while (currentIndex !== 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;
        temporaryValue = a[currentIndex];
        a[currentIndex] = a[randomIndex];
        a[randomIndex] = temporaryValue;
    }
    return a;
}

var interval;


function startTimer() {
    var s = 0; 
    var m = 0; 
    var h = 0;
    interval = setInterval(function () {
        timer.innerHTML = 'Tempo: ' + m + " min " + s + " sec";
        s++;
        if (s == 60) {
            m++;
            s = 0;
        }
        if (m == 60) {
            h++;
            m = 0;
        }
    }, 1000);
}
// una funzione che rimuove la classe active e chiama la funzione startGame()
function startGame() {

   

    clearInterval(interval);
    arrayComparison = [];
    var arrayShuffle = shuffle(arrayAnimali);
    var lista = document.getElementById('griglia');
    while (lista.hasChildNodes()) {
        lista.removeChild(lista.firstChild);
    }

    for ( i = 0; i < 24; i++) {
        var cont = document.createElement('div');
        var element = document.createElement('div');
        element.className = 'icon';
        document.getElementById('griglia')
            .appendChild(cont).appendChild(element);
        element.innerHTML = arrayShuffle[i];
    }

    startTimer();

    var icon = document.getElementsByClassName("icon");
    var icons = [...icon];

    for (var i = 0; i < icons.length; i++) {
        icons[i].addEventListener("click", displayIcon);
        icons[i].addEventListener("click", openModal);
    }

}
// la funzione startGame che pulisce il timer, dichiara un array vuoto, mescola casualmente l'array degli animali
// (var arrayShuffle = shuffle(arrayAnimali);), aggancia il contenitore con id griglia, 
// pulisce tutti gli elementi che eventualmente contiene
// poi fa ciclo per creare i 24 div child -> aggiunge la class e l'elemento dell'array in base all'indice progressivo
// chiama la funzione timer e associa a tutti gli elementi (div) di classe icon l'evento click e le due funzioni definit sotto
 /*
    var icon = document.getElementsByClassName("icon");
    var icons = [...icon];
    è uguale a 
    var icons = document.getElementsByClassName("icon");
    //var icons = [...icon];
    è un operatore che serve per passare un array come argomento:
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax 
    https://www.tutorialspoint.com/es6/es6_operators.htm (cerca spread nella pagina)
    */function displayIcon() {
    var iconsFind = document.getElementsByClassName("find");
    var icon = document.getElementsByClassName("icon");
    var icons = [...icon];
    
    //mette/toglie la classe show
    this.classList.toggle("show");
    //aggiunge l'oggetto su cui ha cliccato all'array del confronto
    arrayComparison.push(this);

    var len = arrayComparison.length;
    //se nel confronto ci sono due elementi
    if (len === 2) {
        //se sono uguali aggiunge la classe find
        if (arrayComparison[0].innerHTML === arrayComparison[1].innerHTML && arrayComparison[0] !== arrayComparison[1]) {
            arrayComparison.forEach(function (elemento) {
                elemento.classList.add("find", "disabled");
            });
            
            arrayComparison = [];
        } else {
            //altrimenti (ha sbagliato) aggiunge solo la classe disabled
            icons.forEach(function(item) {
                item.classList.add('disabled');
            });
            // con il timeout rimuove  la classe show per nasconderli
            setTimeout(function() {
                arrayComparison.forEach(function (elemento) {
                    elemento.classList.remove("show");
                });
               
                icons.forEach(function(item) {
                    item.classList.remove('disabled');
                    for (var i = 0; i < iconsFind.length; i++) {
                        iconsFind[i].classList.add("disabled");
                    }
                });
                arrayComparison = [];
            }, 800);
        }
    }
}

var modal = document.getElementById("modal");
var timer = document.querySelector(".timer");
var iconsFind = document.getElementsByClassName("find");

function openModal() {
    
    if (iconsFind.length == 24) {
        clearInterval(interval);
        modal.classList.add("active");
        document.getElementById("tempoTrascorso").innerHTML = timer.innerHTML;
        closeModal();
    }
}

function closeModal() {
    closeicon.addEventListener("click", function (s) {
        modal.classList.remove("active");
        startGame();
    });
}

function playAgain(){
    modal.classList.remove("active");
    startGame();
}

//una funzione che viene mostrata alla fine quando sono tutte le risposte esatte

// una funzione che nasconde la modale alla fine e riavvia il gioco

// una funzione che calcola il tempo e aggiorna il contenitore sotto


[CSS]


* {
    font-family: 'Lexend', sans-serif;
}
body {
background-image: linear-gradient(190deg,
            hsl(0deg 100% 5%) 0%,
            hsl(357deg 65% 13%) 9%,
            hsl(353deg 78% 20%) 18%,
            hsl(352deg 91% 27%) 27%,
            hsl(353deg 100% 33%) 36%,
            hsl(357deg 100% 41%) 44%,
            hsl(357deg 100% 41%) 53%,
            hsl(353deg 99% 33%) 62%,
            hsl(353deg 88% 27%) 70%,
            hsl(354deg 75% 20%) 80%,
            hsl(357deg 62% 13%) 90%,
            hsl(0deg 91% 4%) 100%);
            background-repeat: no-repeat;
            
            
}
#titolo {
    font-family: 'Lexend', sans-serif;
    width: 100%;
    text-align: center;
    color: #000000;
}

.container {
    
    width: 900px;
    margin: auto;
    background: transparent;
    /* background-image: linear-gradient(190deg,
                hsl(0deg 100% 5%) 0%,
                hsl(3deg 46% 17%) 9%,
                hsl(10deg 53% 27%) 18%,
                hsl(18deg 61% 36%) 27%,
                hsl(26deg 70% 43%) 36%,
                hsl(34deg 84% 47%) 44%,
                hsl(34deg 84% 47%) 53%,
                hsl(26deg 69% 43%) 62%,
                hsl(18deg 60% 36%) 70%,
                hsl(11deg 52% 27%) 80%,
                hsl(3deg 44% 16%) 90%,
                hsl(0deg 91% 4%) 100%); */
    /* box-shadow: 5px 5px 5px 5px  rgba(190, 190, 38, 0.773) */
}


.text-center {
    text-align: center;
}

.icon-grid {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
}



#griglia > div {
    margin: 20px 10px;
    width: 120px;
    height: 120px;
    box-shadow: 5px #c48a38;
    background-image: linear-gradient(190deg,
            hsl(0deg 100% 53%) 0%,
            hsl(356deg 86% 43%) 6%,
            hsl(355deg 78% 34%) 13%,
            hsl(355deg 66% 25%) 19%,
            hsl(357deg 51% 15%) 27%,
            hsl(0deg 38% 3%) 36%,
            hsl(357deg 53% 15%) 47%,
            hsl(355deg 69% 25%) 71%,
            hsl(354deg 82% 33%) 85%,
            hsl(355deg 94% 42%) 93%,
            hsl(0deg 100% 51%) 100%);   
    border: 1px solid #c49538;
    border-radius: 10px;
}

.timer {
    padding: 10px 0;
    color:  yellow;
}

.icon {
    font-size: 80px;
    text-align: center;
    visibility: visible !important;
    opacity: 0;
    width: 100%;
    height: 100%;
    cursor: pointer;
}

.disabled {
    pointer-events: none;
    cursor: wait;
}

.show { 
    visibility: visible;
    opacity: 100;
    opacity: 90;
    visibility: visible;
    animation-name: rotazione-carta;
    animation-duration: .5s;
    background-color: rgb(255, 196, 1);
    border: 1px solid #d62020;
    border-radius: 10px;
}

.find {
    animation-name: indovinato ;
    animation-duration: .5s;
    background-color: rgba(247, 212, 16, 0.56);
    border: 1px solid #6e0000;
    border-radius: 10px;
}

.button {
    color: white;
    font-size: 22px;
    text-align: center;
    margin-top: 10px;
    padding: 10px;
    background-color: #ff0000dc;
    border: 1px solid #000000;
    border-radius: 5px;
}

.button:hover {
    background-color: #ff7b00;
    border: none;
}

#modal {
    display: none;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    background-color: rgba(255, 0, 0, 0.706);
    position: fixed;
    top: 0;
    left: 0;
}

#modal.active {
    display: flex;
}

#modal h2 {
    margin-top: 20px;
}

@media (max-width: 600px) {
    .container {
        width: 400px;
    }
    #griglia>div {
        margin: 5px 5px;
        width: 70px;
        height: 70px;
        
    }
    .icon {
        font-size: 60px;
    }
}

@media (max-width: 420px) {
    .container {
        width: 230px;
    }
    #griglia>div {
        margin: 5px 5px;
        width: 40px;
        height: 40px;
      
    }
    .icon {
        font-size: 20px;
    }
}

@keyframes indovinato {

    /* animazione quando si indovina */
    from {
        transform: scale(0);
    }

    to {
        transform: scale(1.2);
    }
}

@keyframes rotazione-carta {

    /* animazione quando ruota la carta */
    from {
        transform: rotate3d(0, 10, 360, 120deg);
        animation-timing-function: ease-out;
    }

    50% {
        transform: rotate3d(40, 51, 50, 0deg);
        animation-timing-function: ease-out;
    }

    to {
        transform: rotate3d(15, 20, 25, 0deg);
        animation-timing-function: ease-out;
    }
}
>>>>>>> 34509b4cbcf5154a8de980ee908730281ab4a67d


