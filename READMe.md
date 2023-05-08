<html lang="es">

  <head>
    <style>
      body {
        background-image: url('images/giphy2.gif'); 
        background-repeat:repeat;
      }
header {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      background-color: #171616;
      padding: 10px;
      box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
      z-index: 9999;
    }
    h2 {
  text-decoration: none;
}
    #logo {
      height: 50px;
      

      margin-right: 10px;
    }
    #noticias {
      display: flex;
      flex-wrap: wrap;
      text-align: right;
      
      
      margin-top: 5%;
      max-width: 30%;
      float: right;
      margin-right: 2%;
    }

    #barra_busqueda {
      float: right;
      margin-top: 5px;
      margin-bottom: 0px;
      margin-right: 10px;
    }

    .noticias-container {
      display: none;
      flex-basis: 100%; /* Cambiado de 48% a 100% */
      margin-bottom: 5px;
    }
    .noticia {
      position: relative;
    }
    .noticia-link {
      display: block;
      position: relative;
      margin-left: 5px;
    }
    .noticia-imagen {
      display: block;
      width: 100%;
      height: auto;
      object-fit: cover;
    }
    .noticia-info {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      padding: 5px;
      background-color: rgba(0, 0, 0, 0.7);
      color: #fff;
    }
    .noticia-titulo {
      margin: 0;
      font-size: 80%;
    }
    #imagen_noticia{
      height: 100px;
      transition: all 0.3s ease-in-out;
			filter: brightness(100%);
    }
		img:hover {
			filter: brightness(80%);
			box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3);
		}
    .imagen {
			box-shadow: 3px 3px 3px grey;
			padding: 10px;
			border-radius: 5px;
			transition: all 0.3s ease;
			display: inline-block;
		}
		.imagen:hover {
			box-shadow: 5px 5px 5px grey;
			transform: translateY(-5px);
		}
		.dialogo {
			display: none;
			position: fixed;
			top: 50%;
			left: 50%;
			transform: translate(-50%, -50%);
			z-index: 999;
			background-color: white;
			box-shadow: 5px 5px 5px grey;
			border-radius: 5px;
      margin-top: 40px;
		}
		.cerrar {
			position: absolute;
			top: 5px;
			right: 5px;
			font-size: 20px;
			cursor: pointer;
		}
    #search-input{
      height: 40px; 
      width: 1000px;
      border-radius: 5px;
      margin-right: 20px;
      font-size: 25px
    }

      </style>
        <script>
      window.addEventListener('scroll', function() {
        const header = document.querySelector('header');
        if (window.pageYOffset > 100) {
          header.classList.add('scroll');
        } else {
          header.classList.remove('scroll');
        }
      });
    </script>
    

  </head>

  <body>
    <header>
      <a href="https://anilist.co/home"><img src="images/AniList_logo.png" id="logo" ></a>
      <a href="https://twitter.com/home"><img src="images/Logo_of_Twitter.svg.png" height="40px" style="margin-bottom: 5px; margin-right: 5px;"></a>
      <a href="https://anilist.co/home"><img src="images/AniList_logo.png" id="logo"></a>
      <a href="https://anilist.co/home"><img src="images/AniList_logo.png" id="logo"></a>
      <a href="https://anilist.co/home"><img src="images/AniList_logo.png" id="logo"></a>
      <a href="https://anilist.co/home"><img src="images/AniList_logo.png" id="logo"></a>
      <a href="https://anilist.co/home"><img src="images/AniList_logo.png" id="logo"></a>
      <a href="https://anilist.co/home"><img src="images/AniList_logo.png" id="logo"></a>
      <div id="barra_busqueda">

        <form action="https://www.google.com/search" method="get">
          <input type="text" id="search-input" name="q" placeholder="Buscar en Google o escribir una URL"  style="height: 40px; width: 1000px;border-radius: 5px;" >
        </form>
  
      </div>
    </header>


    <div id="noticias"></div>
    <script>
      fetch("https://api.allorigins.win/get?url=https://somoskudasai.com/")
        .then((response) => response.json())
        .then((data) => {
          const parser = new DOMParser();
          const htmlDocument = parser.parseFromString(data.contents, "text/html");
          const noticias = htmlDocument.querySelectorAll("article.ar");
          const noticiasDiv = document.getElementById("noticias");
    let contador = 0;
    noticias.forEach((noticia, i) => {
      if (i >= 9 && i < 19) {
        if (contador % 2 === 0) {
          const noticiasContainer = document.createElement("div");
          noticiasContainer.classList.add("noticias-container");
          noticiasDiv.appendChild(noticiasContainer);
        }
        const titulo = noticia.querySelector("h2.ar-title").innerText;
        const fecha = noticia.querySelector("span.db").innerText;
        const imagenSrc = noticia.querySelector("figure.im img").src;
        const enlace = noticia.querySelector("a.lnk-blk").href;
        const noticiaDiv = document.createElement("div");
        noticiaDiv.classList.add("noticia");
        noticiaDiv.innerHTML = `
          <a href="#" class="noticia-link" onclick="mostrarDialogo(this)"> 

            <img src="${imagenSrc}" alt="${titulo}" class="noticia-imagen">
            <div class="noticia-info">
              <h2 class="noticia-titulo">${titulo}</h2>

            </div>
            <div id="dialogo" class="dialogo">
              <span class="cerrar" onclick="cerrarDialogo()">&times;</span>
              <iframe width="1500" height="800" 
            src="${enlace}" 
            frameborder="0" allowfullscreen></iframe>
            </div>
          </a>
        `;
        const noticiasContainer = noticiasDiv.lastChild;
        noticiasContainer.appendChild(noticiaDiv);
        contador++;
        if (contador % 2 === 0) {
          noticiasContainer.style.display = "flex";
          noticiasContainer.style.justifyContent = "flex-end";
        }
      }
    });
  })
  .catch((error) => {
    console.error(error);
  });

  function abrirDialogo() {
			document.getElementById("dialogo").style.display = "block";
		}
		function cerrarDialogo() {
			document.getElementById("dialogo").style.display = "none";
		}

  
function mostrarDialogo(enlace) {
  const noticia = enlace.closest('.noticia');
  const dialogo = noticia.querySelector('.dialogo');
  dialogo.style.display = 'block';
}


document.addEventListener("keydown", function (event) {
  if (event.key === "Escape") {
    const dialogos = document.querySelectorAll(".dialogo");
    dialogos.forEach(function (dialogo) {
      dialogo.style.display = "none";
    });
  }
});

    </script>
	  
  </body>

</html>


