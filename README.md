# Malla-de-Comercio-Internacional-
Te dejo la Malla de Comercio Internacional UAGRM ,Santa Cruz - Bolivia 
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <title>Malla Comercio Internacional</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      margin: 0;
      padding: 20px;
    }
    h2 {
      color: #003366;
    }
    .semestre {
      margin-bottom: 30px;
      padding: 15px;
      background: #ffffff;
      border-radius: 8px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .materia {
      display: inline-block;
      margin: 8px;
      padding: 10px 14px;
      border-radius: 5px;
      background-color: #ccc; /* Bloqueado */
      color: #333;
      cursor: not-allowed;
      transition: all 0.3s ease;
    }
    .materia.aprobada {
      background-color: #a8f0b0; /* Verde bebé */
      text-decoration: line-through;
      cursor: default;
    }
    .materia.disponible {
      background-color: #b3e5fc; /* Celeste pastel */
      cursor: pointer;
    }
  </style>
</head>
<body>

  <h1>Malla Interactiva - Comercio Internacional (Plan 152-0)</h1>
  <div id="mallaContainer"></div>

  <script>
    const materias = {
      // Semestre 1
      CI5100: { nombre: "Administración Tributaria", prereq: [] },
      EC0100: { nombre: "Introducción a la Economía", prereq: [] },
      IN100: { nombre: "Introducción a la Investigación", prereq: [] },
      MAT100: { nombre: "Cálculo I", prereq: [] },
      CIN100: { nombre: "Fund. del Comercio Internacional", prereq: [] },
      ADG100: { nombre: "Administración General", prereq: [] },

      // Semestre 2
      ADG150: { nombre: "Administración Pública", prereq: ["IN100", "ADG100"] },
      CPA150: { nombre: "Contabilidad I", prereq: ["CI5100"] },
      EC0150: { nombre: "Microeconomía I", prereq: ["MAT100", "EC0100"] },
      MAT150: { nombre: "Cálculo II", prereq: ["MAT100"] },
      CIN150: { nombre: "Comercio Int. Bolivia", prereq: ["CIN100"] },
      ING150: { nombre: "Inglés I", prereq: [] },

      // Semestre 3
      EC0200: { nombre: "Microeconomía II", prereq: ["EC0150"] },
      EST100: { nombre: "Estadística I", prereq: [] },
      CPA200: { nombre: "Costos", prereq: ["CPA150"] },
      CIS200: { nombre: "Geoestrategia Esc. Mundiales", prereq: [] },
      ADG200: { nombre: "Gestión del Talento Humano", prereq: ["ADG150"] },
      ING200: { nombre: "Inglés II", prereq: ["ING150"] },
    };

    const aprobadas = new Set();

    function crearMalla() {
      const container = document.getElementById("mallaContainer");
      container.innerHTML = "";

      const semestres = {};
      for (const sigla in materias) {
        const semestre = sigla.startsWith("CI") || sigla.startsWith("EC") ? 1 :
                         sigla.startsWith("ADG1") || sigla.startsWith("CPA1") ? 2 :
                         sigla.startsWith("EC0") || sigla.startsWith("CPA2") || sigla.startsWith("ADG2") ? 3 : 4;
        if (!semestres[semestre]) semestres[semestre] = [];
        semestres[semestre].push(sigla);
      }

      for (const sem in semestres) {
        const div = document.createElement("div");
        div.className = "semestre";
        div.innerHTML = `<h2>${getNombreSemestre(sem)}</h2>`;
        
        semestres[sem].forEach(sigla => {
          const mat = document.createElement("div");
          mat.className = "materia";
          mat.id = sigla;
          mat.innerText = `${sigla} - ${materias[sigla].nombre}`;
          div.appendChild(mat);
        });

        container.appendChild(div);
      }

      actualizarEstado();
      activarEventos();
    }

    function getNombreSemestre(n) {
      const nombres = ["", "Primer Semestre", "Segundo Semestre", "Tercer Semestre", "Cuarto Semestre"];
      return nombres[n] || `Semestre ${n}`;
    }

    function actualizarEstado() {
      for (const sigla in materias) {
        const materia = materias[sigla];
        const div = document.getElementById(sigla);
        div.className = "materia";

        if (aprobadas.has(sigla)) {
          div.classList.add("aprobada");
        } else if (materia.prereq.every(req => aprobadas.has(req))) {
          div.classList.add("disponible");
        } // else permanece bloqueada
      }
    }

    function activarEventos() {
      for (const sigla in materias) {
        const div = document.getElementById(sigla);
        div.onclick = () => {
          if (div.classList.contains("disponible")) {
            aprobadas.add(sigla);
            actualizarEstado();
          }
        };
      }
    }

    crearMalla();
  </script>
</body>
</html>
