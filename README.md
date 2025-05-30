<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cronograma Sistema de Reservas Hotel - Metodología Cascada</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #2c3e50 0%, #34495e 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.2rem;
            opacity: 0.9;
        }

        .project-info {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            padding: 30px;
            background: #f8f9fa;
        }

        .info-card {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            text-align: center;
            transition: transform 0.3s ease;
        }

        .info-card:hover {
            transform: translateY(-5px);
        }

        .info-card h3 {
            color: #2c3e50;
            margin-bottom: 10px;
            font-size: 1.1rem;
        }

        .info-card p {
            color: #666;
            font-weight: bold;
            font-size: 1.3rem;
        }

        .gantt-container {
            padding: 30px;
            overflow-x: auto;
        }

        .gantt-title {
            text-align: center;
            margin-bottom: 30px;
            color: #2c3e50;
            font-size: 2rem;
            font-weight: 600;
        }

        .gantt-chart {
            min-width: 1200px;
            background: white;
            border: 1px solid #ddd;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        .gantt-header {
            display: grid;
            grid-template-columns: 300px repeat(24, 40px);
            background: linear-gradient(135deg, #3498db, #2980b9);
            color: white;
            font-weight: bold;
            text-align: center;
            padding: 15px 0;
        }

        .gantt-header div {
            padding: 5px;
            border-right: 1px solid rgba(255,255,255,0.2);
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .gantt-row {
            display: grid;
            grid-template-columns: 300px repeat(24, 40px);
            border-bottom: 1px solid #eee;
            transition: background-color 0.3s ease;
        }

        .gantt-row:hover {
            background-color: #f8f9fa;
        }

        .task-name {
            padding: 15px;
            border-right: 1px solid #ddd;
            font-weight: 500;
            display: flex;
            align-items: center;
            background: white;
        }

        .phase-name {
            background: linear-gradient(135deg, #e74c3c, #c0392b);
            color: white;
            font-weight: bold;
        }

        .task-cell {
            border-right: 1px solid #eee;
            position: relative;
            min-height: 50px;
        }

        .task-bar {
            height: 30px;
            margin: 10px 2px;
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 0.8rem;
            font-weight: bold;
            position: relative;
            overflow: hidden;
        }

        .phase-1 { background: linear-gradient(135deg, #e74c3c, #c0392b); }
        .phase-2 { background: linear-gradient(135deg, #f39c12, #e67e22); }
        .phase-3 { background: linear-gradient(135deg, #2ecc71, #27ae60); }
        .phase-4 { background: linear-gradient(135deg, #9b59b6, #8e44ad); }
        .phase-5 { background: linear-gradient(135deg, #34495e, #2c3e50); }

        .task-bar::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.4), transparent);
            animation: shine 2s infinite;
        }

        @keyframes shine {
            0% { left: -100%; }
            100% { left: 100%; }
        }

        .legend {
            padding: 30px;
            background: #f8f9fa;
        }

        .legend-title {
            text-align: center;
            margin-bottom: 20px;
            color: #2c3e50;
            font-size: 1.5rem;
            font-weight: 600;
        }

        .legend-items {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 20px;
        }

        .legend-item {
            display: flex;
            align-items: center;
            gap: 10px;
            background: white;
            padding: 10px 15px;
            border-radius: 25px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
        }

        .legend-color {
            width: 20px;
            height: 20px;
            border-radius: 50%;
        }

        .progress-section {
            padding: 30px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .progress-title {
            text-align: center;
            margin-bottom: 30px;
            font-size: 2rem;
            font-weight: 600;
        }

        .progress-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
        }

        .progress-card {
            background: rgba(255,255,255,0.1);
            padding: 20px;
            border-radius: 15px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.2);
            text-align: center;
        }

        .progress-bar {
            width: 100%;
            height: 10px;
            background: rgba(255,255,255,0.2);
            border-radius: 5px;
            overflow: hidden;
            margin: 15px 0;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #2ecc71, #27ae60);
            border-radius: 5px;
            transition: width 2s ease;
        }

        @media (max-width: 768px) {
            .container {
                margin: 10px;
                border-radius: 10px;
            }
            
            .header h1 {
                font-size: 1.8rem;
            }
            
            .gantt-container {
                padding: 15px;
            }
            
            .project-info {
                grid-template-columns: 1fr;
                padding: 20px;
            }
        }

        .tooltip {
            position: absolute;
            background: #333;
            color: white;
            padding: 8px 12px;
            border-radius: 5px;
            font-size: 0.8rem;
            z-index: 1000;
            opacity: 0;
            transition: opacity 0.3s ease;
            pointer-events: none;
        }

        .task-bar:hover + .tooltip {
            opacity: 1;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Sistema de Reservas para Hotel</h1>
            <p>Cronograma de Proyecto - Metodología Cascada</p>
        </div>

        <div class="project-info">
            <div class="info-card">
                <h3>Duración Total</h3>
                <p>24 Semanas</p>
            </div>
            <div class="info-card">
                <h3>Fases</h3>
                <p>5 Principales</p>
            </div>
            <div class="info-card">
                <h3>Actividades</h3>
                <p>13 Específicas</p>
            </div>
            <div class="info-card">
                <h3>Metodología</h3>
                <p>Cascada</p>
            </div>
        </div>

        <div class="gantt-container">
            <h2 class="gantt-title">Diagrama de Gantt</h2>
            <div class="gantt-chart">
                <div class="gantt-header">
                    <div>Actividad</div>
                    <div>1</div><div>2</div><div>3</div><div>4</div><div>5</div><div>6</div>
                    <div>7</div><div>8</div><div>9</div><div>10</div><div>11</div><div>12</div>
                    <div>13</div><div>14</div><div>15</div><div>16</div><div>17</div><div>18</div>
                    <div>19</div><div>20</div><div>21</div><div>22</div><div>23</div><div>24</div>
                </div>

                <!-- Fase 1: Análisis de Requisitos -->
                <div class="gantt-row">
                    <div class="task-name phase-name">FASE 1: ANÁLISIS DE REQUISITOS</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">1.1 Levantamiento de requisitos</div>
                    <div class="task-cell"><div class="task-bar phase-1">Sem 1-2</div></div>
                    <div class="task-cell"><div class="task-bar phase-1"></div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">1.2 Análisis de viabilidad</div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-1">Sem 3</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">1.3 Documentación de requisitos</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-1">Sem 4</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <!-- Fase 2: Diseño del Sistema -->
                <div class="gantt-row">
                    <div class="task-name phase-name">FASE 2: DISEÑO DEL SISTEMA</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">2.1 Diseño arquitectónico</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-2">Sem 5-6</div></div>
                    <div class="task-cell"><div class="task-bar phase-2"></div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">2.2 Diseño de base de datos</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-2">Sem 7</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">2.3 Diseño de interfaces</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-2">Sem 8-9</div></div>
                    <div class="task-cell"><div class="task-bar phase-2"></div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">2.4 Diseño detallado</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-2">Sem 10</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <!-- Fase 3: Implementación -->
                <div class="gantt-row">
                    <div class="task-name phase-name">FASE 3: IMPLEMENTACIÓN</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">3.1 Desarrollo Backend</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-3">Sem 11</div></div>
                    <div class="task-cell"><div class="task-bar phase-3">12</div></div>
                    <div class="task-cell"><div class="task-bar phase-3">13</div></div>
                    <div class="task-cell"><div class="task-bar phase-3">14</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">3.2 Desarrollo Frontend</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-3">Sem 15</div></div>
                    <div class="task-cell"><div class="task-bar phase-3">16</div></div>
                    <div class="task-cell"><div class="task-bar phase-3">17</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">3.3 Integración de pagos</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-3">Sem 18</div></div>
                    <div class="task-cell"><div class="task-bar phase-3">19</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <!-- Fase 4: Pruebas -->
                <div class="gantt-row">
                    <div class="task-name phase-name">FASE 4: PRUEBAS</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">4.1 Pruebas unitarias</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-4">Sem 20</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">4.2 Pruebas de integración</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-4">Sem 21</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">4.3 Pruebas del sistema</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-4">Sem 22</div></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <!-- Fase 5: Despliegue -->
                <div class="gantt-row">
                    <div class="task-name phase-name">FASE 5: DESPLIEGUE</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">5.1 Preparación entorno producción</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-5">Sem 23</div></div>
                    <div class="task-cell"></div>
                </div>

                <div class="gantt-row">
                    <div class="task-name">5.2 Despliegue y puesta en marcha</div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"></div><div class="task-cell"></div><div class="task-cell"></div>
                    <div class="task-cell"><div class="task-bar phase-5">Sem 24</div></div>
                </div>
            </div>
        </div>

        <div class="legend">
            <h3 class="legend-title">Leyenda de Fases</h3>
            <div class="legend-items">
                <div class="legend-item">
                    <div class="legend-color phase-1"></div>
                    <span>Fase 1: Análisis de Requisitos</span>
                </div>
                <div class="legend-item">
                    <div class="legend-color phase-2"></div>
                    <span>Fase 2: Diseño del Sistema</span>
                </div>
                <div class="legend-item">
                    <div class="legend-color phase-3"></div>
                    <span>Fase 3: Implementación</span>
                </div>
                <div class="legend-item">
                    <div class="legend-color phase-4"></div>
                    <span>Fase 4: Pruebas</span>
                </div>
                <div class="legend-item">
                    <div class="legend-color phase-5"></div>
                    <span>Fase 5: Despliegue</span>
                </div>
            </div>
        </div>

        <div class="progress-section">
            <h3 class="progress-title">Estado del Proyecto (Semana 16)</h3>
            <div class="progress-grid">
                <div class="progress-card">
                    <h4>Análisis de Requisitos</h4>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 100%"></div>
                    </div>
                    <p>100% Completado</p>
                </div>
                <div class="progress-card">
                    <h4>Diseño del Sistema</h4>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 100%"></div>
                    </div>
                    <p>100% Completado</p>
                </div>
                <div class="progress-card">
                    <h4>Implementación</h4>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 65%"></div>
                    </div>
                    <p>65% Completado</p>
                </div>
                <div class="progress-card">
                    <h4>Pruebas</h4>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 0%"></div>
                    </div>
                    <p>0% Completado</p>
                </div>
                <div class="progress-card">
                    <h4>Despliegue</h4>
                    <div class="progress-bar">
                        <div class="progress-fill" style="width: 0%"></div>
                    </div>
                    <p>0% Completado</p>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Animación de las barras de progreso
        window.addEventListener('load', function() {
            const progressBars = document.querySelectorAll('.progress-fill');
            progressBars.forEach(bar => {
                const width = bar.style.width;
                bar.style.width = '0%';
                setTimeout(() => {
                    bar.style.width = width;
                }, 1000);
            });
        });
    </script>
</body>
</html>
