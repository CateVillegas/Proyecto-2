<script>
  import * as d3 from "d3"
  import { onMount } from "svelte"


  // Array donde guardaremos los datos de la encuesta
  let encuesta = []

  let colorGenero = d3.scaleOrdinal()
    .domain(["Femenino", "Masculino", "No binario"])
    .range(["#EEEBE6", "#D4D9E2", "#D1D1D1"])

  let colorMomentoDia = d3.scaleOrdinal()
    .domain(["Mañana", "Tarde", "Noche"])
    .range(["rgba(1, 120, 172, 0.10)", "rgba(1, 120, 172, 0.3)", "rgba(1, 120, 172, 0.7)"])

  let HorasDiarias = d3.scaleOrdinal()
  .domain(["1 - 2 horas", "2 - 3 horas", "3 - 4 horas", "4 - 5 horas"])
  .range(["/images/1 linea.svg", "/images/2 lineas.svg", "/images/3 lineas.svg", "/images/4 lineas.svg"])

  let colorRedSocialMasUsada = d3.scaleOrdinal()
    .domain(["WhatsApp", "Instagram", "YouTube", "Twitter/X", "TikTok", "Books", "Spotify", "Pinterest"])
    .range(["#FFA1A1", "#C9FFA3", "#FFFCAE", "#C0FFF7", "#A0C1FF", "#FFCCFD", "#1DB954", "#DDBBFF"])

  let Motivos = d3.scaleOrdinal()
  .domain(["Comunicación", "Entretenimiento", "Laboral", "Información"])
  .range(["/images/lineas H.png", "/images/puntos.png", "/images/triangulitos.png", "/images/lineas V.png"])

  // Función para cargar los datos CSV y procesarlos
  onMount(() => {
    d3.csv("./data/deportistas.csv", d3.autoType).then(data => {
      console.log(data)
      encuesta = data
    })
  })

  //referencias 
  
  let showReferences = false;

  function toggleReferences() {
  showReferences = !showReferences;
  if (showReferences) {
    // Esto te lleva a la sección de referencias
    location.href = "#referencias";
    document.getElementById('referencias').style.display = 'flex'; 
  } else {
    document.getElementById('referencias').style.display = 'none';
  }
}


</script>

<main>
  <div class="header">
    <img src="public\images\Icono.png" width="100" alt="icono" />
    <h3 class="headline">
      <b> Redes Reveladas: Explorando Tendencias de Uso </b>
    </h3>
    <p class="bajada">Un Análisis Visual de los Hábitos en Redes Sociales, por Estanislao Ríos Zgaib y Caterina Villegas.</p>
  </div>
  

  
  <!-- Contenedor de las entidades -->
  <div class="container">
    
      {#each encuesta as persona}   
        <div class="persona-container" >
          
        
          <svg class="forma-atras" viewBox="0 0 240 214">
            <path d="M129.738 30.5387C91.0014 25.7824 91.426 -25.2671 30.5464 18.3594C-30.3332 61.9859 22.2522 85.9107 16.9376 129.194C4.96067 233.033 62.6556 134.808 89.9465 189.26C117.237 243.713 196.298 196.94 207.566 152.601C208.735 108.808 277.369 90.2885 210.561 40.4625C143.753 -9.36357 161.302 34.4141 129.738 30.5387Z" fill="{colorGenero(persona.Genero)}" />
          </svg>
          <div class= "nombre">
            <p> {persona.nombre} </p>
          </div>
          <svg class="cuadrado-azul" width="50" height="50" >
            <rect width="100%" height="100%" fill="{colorMomentoDia(persona.MomentoDeUso)}" />
          </svg>

          <svg class="forma-principal" >
            <g>
              {#if persona.RedSocialFavorita === 'TikTok' || persona.RedSocialFavorita === 'Youtube'}
                <rect class= "CuadradoForma" x="13" y="12" width="72" height="72" fill="{colorRedSocialMasUsada(persona.redSocialMasUsada)}" stroke="black" stroke-width="2" stroke-dasharray="{persona.RolDeClase === 'alumno' ? '5,5' : 'none'}" filter="url(#external-border)"></rect>
              {:else if persona.RedSocialFavorita === 'Instagram' || persona.RedSocialFavorita === 'Pinterest'}
                <circle class= "CirculoForma" cx="50" cy="50" r="40" fill="{colorRedSocialMasUsada(persona.redSocialMasUsada)}" stroke="black" stroke-width="2" stroke-dasharray="{persona.RolDeClase === 'alumno' ? '5,5' : 'none'}" filter="url(#external-border)"></circle>
              {:else if persona.RedSocialFavorita === 'Facebook' || persona.RedSocialFavorita === 'Twitter/X'}
                <polygon class= "TrianguloForma" points="50,10 10,90 90,90"  fill="{colorRedSocialMasUsada(persona.redSocialMasUsada)}" stroke="black" stroke-width="2" stroke-dasharray="{persona.RolDeClase === 'alumno' ? '5,5' : 'none'}" filter="url(#external-border)"  ></polygon>
              {:else if persona.RedSocialFavorita === 'Books' || persona.RedSocialFavorita === 'WhatsApp' || persona.RedSocialFavorita === 'Spotify'}
                <polygon class= "EstrellaForma"points="55,11 71.5,38.5 99,44 77,66 82.5,93.5 55,82.5 27.5,93.5 33,66 11,44 38.5,38.5" fill="{colorRedSocialMasUsada(persona.redSocialMasUsada)}" stroke="black" stroke-width="2" stroke-dasharray="{persona.RolDeClase === 'alumno' ? '5,5' : 'none'}" filter="url(#external-border)"/>
              {/if}
            </g>
          </svg>

          <svg class="lineas-curvas">
            {#if persona.HorasDeUso}
              <image href={HorasDiarias(persona.HorasDeUso)} 
              alt={"Línea curva para " + persona.HorasDeUso} 
              width="110" />
            {/if}
          </svg>

        
          <div class="linea-recta"></div>

          
          {#each persona.MotivosDeUso.split(';') as motivo}
          <div class="motivos-uso">
            {#if motivo === "Comunicación"}
              <svg class="motivoComContainer"> 
                <image class="motivo com" href={Motivos(motivo)} alt={"Motivo de uso: " + motivo} width="60"/>
              </svg>
            {:else if motivo === "Entretenimiento"}
              <svg class="motivoEntContainer"> 
                <image class="motivo ent" href={Motivos(motivo)} alt={"Motivo de uso: " + motivo} width="60"  />
              </svg>
            {:else if motivo === "Laboral"}
              <svg class="motivoLabContainer"> 
                <image class="motivo lab" href={Motivos(motivo)} alt={"Motivo de uso: " + motivo} width="50"  />
              </svg>
            {:else}
              <svg class="motivoInfoContainer"> 
                <image class="motivo info" href={Motivos(motivo)} alt={"Motivo de uso: " + motivo} width="60"  />
              </svg>
            {/if}
          </div>
        {/each}
        
        </div>
      {/each}
  </div>

  <!-- el botón -->
  <button class="myButton" on:click={toggleReferences}>Mostrar/Ocultar Referencias</button>

  <!-- referecias -->

  <div id="referencias" class="referencias" style="display: {showReferences ? 'flex' : 'none'};">

    <div class="containerEdad">
      <svg class="Edad">
        <image class="RolClase" xlink:href="public\images\Edad.png" />
      </svg>
      <p class="tituloRC">Rol de la Clase</p>
      <p class="Alumno">Alumno</p>
      <p class="Profesor">Profesor</p>
    </div>

    <div class="containerGenero"> 
      <svg class="Gen">
        <image class="IGenero" xlink:href=public\images\Genero.png/>
      </svg>
      <p class="tituloG">Género</p>
      <p class="Mujer">Mujer</p>
      <p class="Hombre">Hombre</p>
      <p class="NoBin">No binario</p>
    </div>

    <div class="containerRedSocialMasUsada"> 
      <svg class="RedSocialMasUsada">
        <image class="RSMasUsada" xlink:href=public\images\RedSocialMasUsada.png/>
      </svg>
      <p class="tituloRS">Red social más usada</p>
      <p class="Instagram">Instagram</p>
      <p class="Whatsapp">Whatsapp</p>
      <p class="Twitter">Twitter</p>
      <p class="Youtube">Youtube</p>
      <p class="TikTok">TikTok</p>
      <p class="Books">Books</p>
      <p class="Pinterest">Pinterest</p>
    </div>

    <div class="containerMotivosUso"> 
      <svg class="MotivosUso">
        <image class="MUsos" xlink:href="public\images\MotivosDeUso.png"/>
      </svg>
      <p class="tituloMotivosU">Motivos de uso</p>
      <p class="Lab">Laboral</p>
      <p class="Info">Información</p>
      <p class="Com">Comunicación</p>
      <p class="Ent">Entretenimiento</p>
    </div>

    <div class="containerMomentoUso"> 
      <svg class="MomentoUso">
        <image class="MUso" xlink:href=public\images\MomentoDeUso.png/>
      </svg>
      <p class="tituloMU">Momento que más usas redes </p>
      <p class="Mañana">Mañana</p>
      <p class="Tarde">Tarde</p>
      <p class="Noche">Noche</p>
     
    </div>

    <div class="containerHorasDeUsoDia"> 
      <svg class="HorasDeUsoDia">
        <image class="HorasDia" xlink:href=public\images\HorasDeUso.png/>
      </svg>
      <p class="tituloHU">Horas de uso al día</p>
      <p class="horas1-2">1-2 horas</p>
      <p class="horas2-3">2-3 horas</p>
      <p class="horas3-4">3-4 horas</p>
      <p class="horas4-5">4-5 horas</p>
      
    </div>

    <div class="containerFormas"> 
      <svg class="FormasPrinc">
        <image class="FormasP" xlink:href=public\images\FormasProtagonistas.png/>
      </svg>
      <p class="tituloFP"> Red social favorita</p>
      <p class="Video"><strong>Video</strong> TikTok Youtube</p>
      <p class="Imagen"><strong>Imagen</strong> Pinterest Instagram</p>
      <p class="Noticias"><strong>Noticias</strong> FaceBook Twitter</p>
      <p class="Otros"><strong>Otros</strong> Books WhatsApp</p>
    </div>

  </div>
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Briem+Hand:wght@100..900&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Rock+Salt&family=Sedgwick+Ave&display=swap');
  @import url('https://fonts.googleapis.com/css2?family=Briem+Hand:wght@100..900&family=Chelsea+Market&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Rock+Salt&family=Sedgwick+Ave&display=swap');

  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-top: 0;
    margin-bottom: 80px;
  }

  .headline {
    width: 70%;
    line-height: 1.4; 
    font-family: "Chelsea Market", system-ui;
    font-size: 35px;
    color: black;
    font-weight: 900;
    text-align: center;
    margin: 20px;
  }

  .bajada {
    font-size: 14px;
    font-weight: normal;
    text-align: center;
    margin: 10px;
    color: black;
    font-family: 'Poppins', sans-serif;
    transform: translateY(-50%);
  }

  .container {
  /* background-color: #D3CEC6; */
    display: flex;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 10000px;
    gap: 20px;
    margin-bottom: 100px;
    border-radius: 1%;
    transform: translateY(-1.3%);
  }

  .persona-container {
    background-color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 10px;
    width: 190px;
    height: 220px;
    border-width: 10px ;
    border-radius: 10%;
  }

  .forma-principal {
    width: 130px; 
    height: 130px; 
    padding: 5px; 
    box-sizing: border-box; 
    flex-direction: row;
    align-items: center;
    position: absolute;
    z-index: 3; 
  }

  .forma-principal rect{
    transform: translate(0%, 0%) scale(1.2);
  }

  .forma-principal circle{
    transform: translate(0%, 2%) scale(1.2);
  }

  .forma-principal .EstrellaForma {
    border-radius: 100%;
    transform: translate(-5%, 0%) scale(1.2); 
  }

  .forma-principal .TrianguloForma {
    border-radius: 100%;
    transform: translate(0%, 0%) scale(1.2); 
  }

  .forma-atras {
    position: absolute;
    width: 116px; 
    height: 150px; 
    padding: 5px; 
    box-sizing: border-box;
    z-index: 2; 
    transform: rotate(100deg);
    transform: translate(-10%, -10%)
  }

  .cuadrado-azul {
    position: absolute;
    width: 75px;
    height: 80px; 
    padding: 5px; 
    border-radius: 10%;
    box-sizing: border-box;
    z-index: 1; 
    transform: translate(44%, 49%)
  }

  .lineas-curvas{
    width: 130px; 
    height: 118px; 
    padding: 5px; 
    box-sizing: border-box;
    z-index: 3;
  }

  .linea-recta {  
    width: 1.34px;
    height: 120px; 
    background-color: black;
    transform: translate(-65px, 14px);
    z-index: 4;
}

  .nombre{
    color: black;
    font-family: 'Poppins', sans-serif;
    position: absolute;
    width: 145px;
    height: 20px; 
    padding: 5px; 
    font-size: 13px;
    transform: translate(2.5px, -75px);
    display: flex;
    align-items: center; /* Centra verticalmente */
    justify-content: center; /* Centra horizontalmente */
    margin-right: 5px;
  }

  .motivos-uso {
    width: 130px; 
    height: 140px; 
    padding: 5px; 
    box-sizing: border-box; 
    flex-direction: row;
    align-items: center;
    position: absolute;
    justify-content: center;
    transform: translate(0%,2.9%);
    z-index: 5;
  }

  .motivos-uso .motivoComContainer {
    width: 51.5px; 
    height:25px; 
    position: relative;
    padding: 5px; 
    box-sizing: border-box; 
    align-items: center;
    justify-content: center;
    transform: translate(106%,86%);
    z-index: 6;
  }

  .motivos-uso .motivoEntContainer {
    width: 56px; 
    height:25px; 
    position: relative;
    padding: 5px; 
    box-sizing: border-box; 
    align-items: center;
    justify-content: center;
    transform: translate(17%,45%);
    z-index: 6;
  }

  .motivos-uso .motivoLabContainer {
    width: 52px; 
    height:24px; 
    position: relative;
    padding: 5px; 
    box-sizing: border-box; 
    align-items: center;
    justify-content: center;
    transform: translate(105%,166%);
    z-index: 6;
  }

  .motivos-uso .motivoInfoContainer {
    width: 56px;  
    height:23px;  
    position: relative;
    padding: 5px; 
    box-sizing: border-box; 
    align-items: center;
    justify-content: center;
    transform: translate(17%,144%);
    z-index: 6;
  }

  /* Codigo referencias */
  .referencias{
    background-color: rgb(255, 255, 255);
    display: flex;
    justify-content: center;
    align-items: center;
    margin: auto;
    flex-wrap: wrap;
    width: 80%;
    gap: 0px;
    margin-bottom: 100px;
    border-radius: 1%;
    flex-direction: row;
    border-style: solid;
    border-color: #FFA1A1;
    transform: translateY(-6%);
  }
  

  /* Edad */
  .containerEdad {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 30px;
    border-width: 3px;
    box-sizing: border-box;
    flex-direction: row;
  }
  .Edad {
    width: 110px;
    height: 100px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-width: 0px;
    box-sizing: border-box;
    padding: 3px; 
    margin: 10px;
  }
  .RolClase {
    width: 100px;
    height: 100px;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
  }
  .tituloRC{
    position: absolute;
    transform: translate(0%,-300%);
    font-family: 'Poppins';
    color: black;
  }
  .Alumno{
    position: absolute;
    transform: translate(-90%,280%);
    font-family: 'Poppins';
    color: black;
  }
  .Profesor{
    position: absolute;
    transform: translate(90%,280%);
    font-family: 'Poppins';
    color: black;
  }

/* Motivo de usos de redes */
  .containerMotivosUso {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 30px;
    box-sizing: border-box;
    flex-direction: row;
  }
  .MotivosUso {
    width: 300px;
    height: 80px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-width: 0px;
    box-sizing: border-box;
    padding: 3px; 
    margin: 10px;
  }
  .MUsos {
    width: 260px;
    height: 75px;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    transform: translateX(2.5%);
  }
  .tituloMotivosU{
    position: absolute;
    transform: translate(0%,-250%);
    font-family: 'Poppins';
    color: black;
  }
  .Info{
    position: absolute;
    transform: translate(-80%,280%);
    font-family: 'Poppins';
    color: black;
    font-size: 10px;
    font-style: bold;
  }
  .Com{
    position: absolute;
    transform: translate(35%,280%);
    font-family: 'Poppins';
    color: black;
    font-size: 10px;
    font-style: bold;
  }
  .Lab{
    position: absolute;
    transform: translate(-320%,280%);
    font-family: 'Poppins';
    color: black;
    font-size: 10px;
    font-style: bold;
  }
  .Ent{
    position: absolute;
    transform: translate(134%,280%);
    font-family: 'Poppins';
    color: black;
    font-size: 10px;
    font-style: bold;
  }

  /* Genero */
  .containerGenero {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 30px;
    box-sizing: border-box;
  }
  .Gen {
    width: 260px;
    height: 100px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-width: 0px;
    box-sizing: border-box;
    padding: 3px; 
    margin: 10px;
    transform: translate(0%, -3%);
  }
  .IGenero {
    width: 276px;
    height: 175px;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    transform: translate(-3%, -40%);
  }
  .tituloG{
    position: absolute;
    transform: translate(0%,-300%);
    font-family: 'Poppins';
    color: black;
  }
  .Mujer{
    position: absolute;
    transform: translate(-250%,280%);
    font-family: 'Poppins';
    color: black;
    font-size: 12px;
  }
  .Hombre{
    position: absolute;
    transform: translate(0%,280%);
    font-family: 'Poppins';
    color: black;
    font-size: 12px;
  }
  .NoBin{
    position: absolute;
    transform: translate(150%,280%);
    font-family: 'Poppins';
    color: black;
    font-size: 12px;
  }

  /* Red Social Mas Usada */
  .containerRedSocialMasUsada {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 30px;
    box-sizing: border-box;
  }
  .RedSocialMasUsada  {
    width: 260px;
    height: 103px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-width: 0px;
    box-sizing: border-box;
    padding: 3px; 
    margin: 10px;
  }
  .RSMasUsada {
    width: 205px;
    height: 175px;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    transform: translate(0%, -42%);
  }
  .tituloRS{
    position: absolute;
    transform: translate(0%,-300%);
    font-family: 'Poppins';
    color: black;
  }
  .Instagram{
    position: absolute;
    transform: translate(-110%,-190%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .Whatsapp{
    position: absolute;
    transform: translate(38%,-190%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .Twitter{
    position: absolute;
    transform: translate(280%,-190%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }

  .Youtube{
    position: absolute;
    transform: translate(-153%,20%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .TikTok{
    position: absolute;
    transform: translate(28%,20%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .Books{
    position: absolute;
    transform: translate(300%,20%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .Pinterest{
    position: absolute;
    transform: translate(40%,220%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }

  /* Horas de Uso al Dia */
  .containerHorasDeUsoDia {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 30px;
    box-sizing: border-box;
  }
  .HorasDeUsoDia {
    width: 290px;
    height: 100px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-width: 0px;
    box-sizing: border-box;
    padding: 3px; 
    margin: 10px;
    transform: translate(0%, -3%);
  }
  .HorasDia {
    width: 270px;
    height: 175px;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    transform: translate(0%, -40%);
  }
  .tituloHU{
    position: absolute;
    transform: translate(0%,-300%);
    font-family: 'Poppins';
    color: black;
  }
  .horas1-2{
    position: absolute;
    transform: translate(-225%,300%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .horas2-3{
    position: absolute;
    transform: translate(-90%,300%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .horas3-4{
    position: absolute;
    transform: translate(35%,300%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .horas4-5{
    position: absolute;
    transform: translate(175%,300%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }

  /* Formas Principales */
  .containerFormas {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 30px;
    box-sizing: border-box;
  }
  .FormasPrinc {
    width: 260px;
    height: 100px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-width: 0px;
    box-sizing: border-box;
    padding: 3px; 
    margin: 10px;
    transform: translate(0%, -3%);
  }

  .FormasP {
    width: 260px;
    height: 175px;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    transform: translate(0%, -36%);
  }
  .tituloFP{
    position: absolute;
    transform: translate(0%,-300%);
    font-family: 'Poppins';
    color: black; 
  }
  .Video{
    position: absolute;
    width: 60px;
    transform: translate(-160%,100%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .Imagen{
    width: 60px;
    position: absolute;
    transform: translate(-43%,100%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .Noticias{
    width: 60px;
    position: absolute;
    transform: translate(70%,100%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }
  .Otros{
    width: 60px;
    position: absolute;
    transform: translate(190%,100%);
    font-family: 'Poppins';
    color: black;
    font-size: 11px;
  }

  /* Momento que mas usas redes */
  .containerMomentoUso {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 30px;
    box-sizing: border-box;
  }
  .MomentoUso {
    width: 260px;
    height: 100px;
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    border-width: 0px;
    box-sizing: border-box;
    padding: 3px; 
    margin: 10px;
    transform: translate(0%, -3%);
  }
  .MUso {
    width: 250px;
    height: 175px;
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    box-sizing: border-box;
    transform: translate(0%, -42%);
  }
  .tituloMU{
    position: absolute;
    transform: translate(0%,-250%);
    font-family: 'Poppins';
    color: black;
  }
  .Mañana{
    position: absolute;
    transform: translate(-180%,200%);
    font-family: 'Poppins';
    color: black;
    font-size: 12px;
  }
  .Tarde{
    position: absolute;
    transform: translate(0%,200%);
    font-family: 'Poppins';
    color: black;
    font-size: 12px;
  }
  .Noche{
    position: absolute;
    transform: translate(220%,200%);
    font-family: 'Poppins';
    color: black;
    font-size: 12px;
  }

  .myButton {
    background-color: #FFA1A1; 
    color: rgb(0, 0, 0); 
    border: none;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 12px;
    font-family: 'Poppins';
    margin: 4px 2px;
    cursor: pointer;
    transform: translate(0%, -200%);
    }
</style>


  