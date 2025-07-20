<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Malla Derecho Interactiva</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9f9f9;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #333;
    }
    .semestre {
      margin-bottom: 25px;
      border: 1px solid #ddd;
      border-radius: 6px;
      padding: 15px;
      background: #fff;
    }
    .ramo {
      display: block;
      margin: 5px 0;
    }
    .bloqueado {
      color: #aaa;
    }
    .desbloqueado {
      color: #0a5;
      font-weight: bold;
    }
    .aprobado {
      background-color: #d0f0c0;
      padding: 3px;
      border-radius: 3px;
    }
  </style>
</head>
<body>
  <h1>Malla Interactiva - Derecho UOH</h1>
  <div id="malla-container"></div>

  <script>
    const malla = {
      "1° Semestre": {
        "Fundamentos del derecho público": [],
        "Fundamentos del derecho privado": []
      },
      "2° Semestre": {
        "Teoría del sistema jurídico": [],
        "Derecho y Economía": ["Fundamentos del derecho privado"]
      },
      "3° Semestre": {
        "Privado 1": ["Fundamentos del derecho privado"],
        "Penal 1": ["Fundamentos del derecho privado"]
      },
      "4° Semestre": {
        "Privado 2": ["Privado 1"],
        "Penal 2": ["Penal 1"]
      },
      "5° Semestre": {
        "Privado 3": ["Privado 2"],
        "Penal 3": ["Penal 2"]
      },
      "6° Semestre": {
        "Privado 4": ["Privado 3"],
        "Penal 4": ["Penal 3"]
      },
      "7° Semestre": {
        "Privado 5": ["Privado 4"]
      },
      "8° Semestre": {
        "Privado 6": ["Privado 5"],
        "Filosofía del Derecho e Interpretación Jurídica": []
      },
      "9° Semestre": {
        "Clínica Jurídica": ["Privado 6", "Penal 4"],
        "Especialización": []
      },
      "10° Semestre": {
        "Resolución de Casos": ["Privado 6"],
        "Instituciones fundamentales del Derecho Procesal": []
      }
    };

    function renderMalla() {
      const container = document.getElementById("malla-container");
      Object.entries(malla).forEach(([semestre, ramos]) => {
        const div = document.createElement("div");
        div.className = "semestre";
        div.innerHTML = `<h3>${semestre}</h3>`;
        Object.entries(ramos).forEach(([ramo, prereqs]) => {
          const id = ramo.replace(/\s+/g, "_");
          const label = document.createElement("label");
          label.className = "ramo bloqueado";
          label.innerHTML = `
            <input type="checkbox" id="${id}" onchange="actualizarMalla()"> ${ramo}
          `;
          label.title = prereqs.length > 0 ? `Prerrequisitos: ${prereqs.join(", ")}` : "Sin prerrequisitos";
          div.appendChild(label);
        });
        container.appendChild(div);
      });
      actualizarMalla();
    }

    function actualizarMalla() {
      const aprobados = new Set();
      document.querySelectorAll("input[type=checkbox]").forEach(input => {
        if (input.checked) aprobados.add(input.id.replace(/_/g, " "));
      });

      Object.entries(malla).forEach(([semestre, ramos]) => {
        Object.entries(ramos).forEach(([ramo, prereqs]) => {
          const id = ramo.replace(/\s+/g, "_");
          const input = document.getElementById(id);
          const label = input.parentElement;

          if (input.checked) {
            label.className = "ramo aprobado";
            input.disabled = false;
          } else if (prereqs.every(r => aprobados.has(r))) {
            label.className = "ramo desbloqueado";
            input.disabled = false;
          } else {
            label.className = "ramo bloqueado";
            input.disabled = true;
            input.checked = false;
          }
        });
      });
    }

    window.onload = renderMalla;
  </script>
</body>
</html>
