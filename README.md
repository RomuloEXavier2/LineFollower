<!DOCTYPE html>
<html lang="pt-BR" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Guia Interativo para Robôs Segue-Linha de Competição</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Calm Harmony -->
    <!-- Application Structure Plan: A thematic, dashboard-style single-page application. The structure prioritizes user exploration and comparison over mirroring the report's linear format. Key sections include an interactive Architecture Matrix, a tabbed Component Explorer with comparison charts, an interactive PID tuning simulator, and a filterable gallery of open-source projects. This design was chosen to make the dense technical information more digestible and engaging, allowing users to compare trade-offs and understand concepts visually. -->
    <!-- Visualization & Content Choices: Report Info -> Goal -> Viz/Presentation Method -> Interaction -> Justification -> Library/Method. 1. Architecture Comparison (Table 1) -> Compare -> Interactive Diagram + Bar Chart -> Click to highlight & update info panel/chart -> More engaging than a static table, visualizes trade-offs -> HTML/CSS/JS + Chart.js. 2. MCU Comparison -> Compare -> Radar Chart -> Static chart -> Provides a quick multi-dimensional visual summary of MCU strengths -> Chart.js. 3. PID Tuning -> Explain Process -> Interactive Sliders + Line Chart -> User adjusts Kp/Kd sliders, chart animates response -> Makes an abstract control theory concept tangible and intuitive -> Chart.js + JS. 4. Open-Source Projects -> Provide Resources -> Filterable Card Gallery -> User clicks filters (STM32, ESP32), gallery updates -> Efficient way to browse relevant projects -> HTML/CSS/JS. All visuals use Canvas, HTML/CSS, or Unicode, adhering to constraints. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #F8F7F4;
            color: #2c3e50;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
        .nav-link {
            transition: color 0.3s, border-bottom-color 0.3s;
            border-bottom: 2px solid transparent;
        }
        .nav-link:hover, .nav-link.active {
            color: #4DB6AC;
            border-bottom-color: #4DB6AC;
        }
        .tab-button {
            transition: all 0.3s;
        }
        .tab-button.active {
            background-color: #4DB6AC;
            color: white;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
        }
        .card {
            transition: transform 0.3s, box-shadow 0.3s;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -2px rgb(0 0 0 / 0.1);
        }
        .architecture-card {
            cursor: pointer;
            border: 2px solid transparent;
            transition: all 0.3s ease;
        }
        .architecture-card.selected {
            border-color: #4DB6AC;
            transform: scale(1.05);
            box-shadow: 0 8px 25px rgba(77, 182, 172, 0.2);
        }
    </style>
</head>
<body class="antialiased">

    <header class="bg-white/80 backdrop-blur-lg shadow-sm sticky top-0 z-50">
        <nav class="container mx-auto px-6 py-3">
            <div class="flex justify-between items-center">
                <h1 class="text-xl md:text-2xl font-bold text-gray-800">Robô Segue-Linha Pro</h1>
                <div class="hidden md:flex items-center space-x-6">
                    <a href="#overview" class="nav-link pb-1">Visão Geral</a>
                    <a href="#architectures" class="nav-link pb-1">Arquiteturas</a>
                    <a href="#components" class="nav-link pb-1">Componentes</a>
                    <a href="#control" class="nav-link pb-1">Controle</a>
                    <a href="#projects" class="nav-link pb-1">Projetos</a>
                </div>
                <div class="md:hidden">
                    <select id="mobile-nav" class="bg-gray-200 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-[#4DB6AC] focus:border-[#4DB6AC] block w-full p-2.5">
                        <option value="#overview">Visão Geral</option>
                        <option value="#architectures">Arquiteturas</option>
                        <option value="#components">Componentes</option>
                        <option value="#control">Controle</option>
                        <option value="#projects">Projetos</option>
                    </select>
                </div>
            </div>
        </nav>
    </header>

    <main class="container mx-auto px-6 py-8 md:py-12">
        
        <section id="overview" class="mb-16 text-center">
            <h2 class="text-3xl md:text-4xl font-bold mb-4">Engenharia de um Campeão</h2>
            <p class="max-w-3xl mx-auto text-lg text-gray-600 mb-8">
                Construir um robô seguidor de linha vencedor vai além de montar peças. É um exercício de engenharia que equilibra mecânica, eletrônica e software. Esta aplicação interativa destila o conhecimento de projetos campeões para guiar você na jornada de criar uma máquina de alta performance, desde a escolha da arena até a implementação de algoritmos avançados.
            </p>
            <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-6">
                <div class="card bg-white p-6 rounded-xl shadow-md">
                    <h3 class="font-bold text-xl mb-2">All Japan Contest</h3>
                    <p class="text-gray-600">O ápice da velocidade e estratégia. Pistas longas e complexas que recompensam o mapeamento e o controle preditivo.</p>
                </div>
                <div class="card bg-white p-6 rounded-xl shadow-md">
                    <h3 class="font-bold text-xl mb-2">Robotex International</h3>
                    <p class="text-gray-600">Um desafio de robustez. Pistas com obstáculos, rampas e falhas que exigem algoritmos adaptáveis e tolerantes a falhas.</p>
                </div>
                <div class="card bg-white p-6 rounded-xl shadow-md">
                    <h3 class="font-bold text-xl mb-2">RobotChallenge</h3>
                    <p class="text-gray-600">O campo de provas global. Foco na confiabilidade e na capacidade de ajuste fino (*tuning*) no local da competição.</p>
                </div>
                <div class="card bg-white p-6 rounded-xl shadow-md">
                    <h3 class="font-bold text-xl mb-2">RoboCore (Brasil)</h3>
                    <p class="text-gray-600">A potência latina. Inova com a modalidade "Perseguição", exigindo velocidade e consistência em competição direta.</p>
                </div>
            </div>
        </section>

        <section id="architectures" class="mb-16">
            <h2 class="text-3xl md:text-4xl font-bold text-center mb-4">Arquiteturas de Sistema</h2>
            <p class="max-w-3xl mx-auto text-lg text-gray-600 text-center mb-12">
                A escolha da arquitetura, a combinação entre o "cérebro" (processador) e os "olhos" (sensores), é a decisão mais fundamental do projeto. Ela define os limites de performance, a complexidade do software e a estratégia de controle. Explore e compare as arquiteturas mais comuns para entender suas vantagens e desvantagens.
            </p>
            <div class="lg:grid lg:grid-cols-3 lg:gap-8 items-start">
                <div class="lg:col-span-1 space-y-4 mb-8 lg:mb-0">
                    <div id="arch-card-1" class="architecture-card bg-white p-5 rounded-xl shadow-lg selected" onclick="selectArchitecture(1)">
                        <h3 class="font-bold text-lg">Array IR (Digital) + STM32/ESP32</h3>
                        <p class="text-sm text-gray-500">Velocidade pura em pistas bem definidas.</p>
                    </div>
                    <div id="arch-card-2" class="architecture-card bg-white p-5 rounded-xl shadow-lg" onclick="selectArchitecture(2)">
                        <h3 class="font-bold text-lg">Array IR (Analógico) + STM32/ESP32</h3>
                        <p class="text-sm text-gray-500">Controle suave e preciso.</p>
                    </div>
                    <div id="arch-card-3" class="architecture-card bg-white p-5 rounded-xl shadow-lg" onclick="selectArchitecture(3)">
                        <h3 class="font-bold text-lg">Câmera + Raspberry Pi</h3>
                        <p class="text-sm text-gray-500">Robustez para pistas complexas.</p>
                    </div>
                     <div id="arch-card-4" class="architecture-card bg-white p-5 rounded-xl shadow-lg" onclick="selectArchitecture(4)">
                        <h3 class="font-bold text-lg">Câmera + FPGA</h3>
                        <p class="text-sm text-gray-500">Performance de tempo real inigualável.</p>
                    </div>
                </div>
                <div class="lg:col-span-2 bg-white p-6 rounded-2xl shadow-xl">
                    <div id="architecture-details">
                    </div>
                    <div class="chart-container mt-6 h-64 md:h-80">
                        <canvas id="architectureChart"></canvas>
                    </div>
                </div>
            </div>
        </section>

        <section id="components" class="mb-16">
            <h2 class="text-3xl md:text-4xl font-bold text-center mb-4">Anatomia de um Campeão</h2>
             <p class="max-w-3xl mx-auto text-lg text-gray-600 text-center mb-12">
                A excelência de um robô de competição não está em uma única peça, mas na sinergia entre todos os seus componentes. Cada escolha, do microcontrolador ao tipo de pneu, impacta diretamente a performance. Navegue pelas abas para explorar os componentes críticos que formam o "corpo" da máquina.
            </p>
            <div id="tabs-container" class="mb-6 flex flex-wrap justify-center gap-2">
                <button class="tab-button active px-4 py-2 rounded-lg font-semibold" onclick="openTab(event, 'Cerebro')">🧠 Cérebro (MCUs)</button>
                <button class="tab-button px-4 py-2 rounded-lg font-semibold" onclick="openTab(event, 'Olhos')">👀 Olhos (Sensores)</button>
                <button class="tab-button px-4 py-2 rounded-lg font-semibold" onclick="openTab(event, 'Musculos')">💪 Músculos (Drivetrain)</button>
                <button class="tab-button px-4 py-2 rounded-lg font-semibold" onclick="openTab(event, 'Esqueleto')">🔩 Esqueleto (Chassi)</button>
            </div>

            <div id="Cerebro" class="tab-content">
                <div class="grid md:grid-cols-2 gap-8 items-center">
                    <div>
                        <h3 class="text-2xl font-bold mb-4">Microcontroladores: O Poder de Decisão</h3>
                        <p class="text-gray-600 mb-4">O MCU define a velocidade de processamento e a complexidade dos algoritmos que o robô pode executar. A escolha varia desde plataformas de 8-bits para iniciantes até poderosos processadores de 32-bits para controle em tempo real de alta frequência, essencial para estabilidade em altas velocidades.</p>
                        <ul class="space-y-3 list-disc list-inside text-gray-700">
                            <li><b>Arduino (8-bit):</b> Excelente para aprendizado, mas limitado para alta performance.</li>
                            <li><b>ESP32 (32-bit):</b> Ótimo equilíbrio entre poder e facilidade, com Wi-Fi/Bluetooth para tuning remoto.</li>
                            <li><b>STM32 (32-bit):</b> A escolha dos campeões para velocidade pura, capaz de executar loops de controle ultra-rápidos.</li>
                            <li><b>Raspberry Pi (SoC):</b> Necessário para processamento de imagem em robôs baseados em visão.</li>
                        </ul>
                    </div>
                    <div class="chart-container h-80 md:h-96">
                        <canvas id="mcuChart"></canvas>
                    </div>
                </div>
            </div>

            <div id="Olhos" class="tab-content hidden">
                 <div class="bg-white p-8 rounded-xl shadow-lg">
                    <h3 class="text-2xl font-bold mb-4">Sensores: A Percepção do Mundo</h3>
                    <p class="text-gray-600 mb-6">A qualidade da percepção define a precisão do robô. A escolha vai de arrays de sensores infravermelhos, a espinha dorsal da maioria dos projetos, a sistemas de visão computacional, a fronteira da navegação inteligente.</p>
                    <div class="grid md:grid-cols-2 gap-6">
                        <div>
                            <h4 class="font-bold text-xl mb-2">Arrays de Sensores IR</h4>
                            <p class="mb-2">Medem a luz infravermelha refletida para detectar a linha preta. A quantidade e o tipo (analógico ou digital) impactam a resolução e a velocidade da leitura.</p>
                            <ul class="space-y-2 list-disc list-inside">
                                <li><b>Analógico:</b> Fornece alta resolução posicional através de média ponderada, ideal para controle PID suave.</li>
                                <li><b>Digital:</b> Leitura mais rápida, favorecido em robôs de alta velocidade com MCUs potentes.</li>
                            </ul>
                        </div>
                        <div>
                            <h4 class="font-bold text-xl mb-2">Sistemas de Visão (Câmera)</h4>
                            <p class="mb-2">Utilizam uma câmera e processamento de imagem (OpenCV) para "ver" a pista. Permitem antecipar curvas e lidar com percursos complexos (intersecções, falhas).</p>
                             <ul class="space-y-2 list-disc list-inside">
                                <li><b>Vantagem:</b> Robustez e inteligência de navegação.</li>
                                <li><b>Desvantagem:</b> Maior latência, complexidade e custo.</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>

            <div id="Musculos" class="tab-content hidden">
                <div class="bg-white p-8 rounded-xl shadow-lg">
                    <h3 class="text-2xl font-bold mb-4">Drivetrain: A Força Bruta</h3>
                    <p class="text-gray-600 mb-6">O trem de força traduz os comandos do cérebro em movimento. A otimização envolve uma troca cuidadosa entre velocidade máxima e torque, além da máxima eficiência na entrega de potência.</p>
                    <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-4">
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-lg">Motores</h4>
                            <p>Motores DC com caixa de engrenagens metálicas são o padrão. A relação de engrenagem (ex: 10:1 vs 30:1) é uma troca entre velocidade e torque.</p>
                        </div>
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-lg">Drivers</h4>
                            <p>Drivers baseados em MOSFET (ex: TB6612FNG) são cruciais por sua alta eficiência, entregando mais potência aos motores.</p>
                        </div>
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-lg">Rodas e Pneus</h4>
                            <p>O diâmetro da roda afeta a velocidade. A aderência do pneu (silicone, fitas) é o fator mais crítico para o desempenho em curvas.</p>
                        </div>
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-lg">Bateria</h4>
                            <p>Baterias LiPo são universais pela alta densidade de energia e capacidade de fornecer picos de corrente para aceleração.</p>
                        </div>
                    </div>
                </div>
            </div>

            <div id="Esqueleto" class="tab-content hidden">
                 <div class="bg-white p-8 rounded-xl shadow-lg">
                    <h3 class="text-2xl font-bold mb-4">Chassi: A Fundação Estrutural</h3>
                    <p class="text-gray-600 mb-6">O chassi deve ser leve e rígido, com um centro de gravidade baixo para máxima estabilidade. O design impacta diretamente a agilidade e a capacidade de resposta do robô.</p>
                    <div class="grid md:grid-cols-2 gap-6">
                        <div>
                            <h4 class="font-bold text-xl mb-2">Design "Hammerhead"</h4>
                            <p>Um design icônico em robôs de alta performance. O array de sensores é montado em uma extensão à frente das rodas. Isso dá ao sistema um "aviso prévio" da curva, compensando a latência do sistema eletrônico e permitindo que o robô reaja a tempo em altas velocidades.</p>
                        </div>
                        <div>
                            <h4 class="font-bold text-xl mb-2">Materiais</h4>
                            <p>A impressão 3D (PETG, ABS, compósitos) revolucionou a construção, permitindo a criação de geometrias complexas e otimizadas para leveza e rigidez, com suportes de componentes integrados para reduzir o peso total.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="control" class="mb-16">
            <h2 class="text-3xl md:text-4xl font-bold text-center mb-4">A Alma da Máquina: Controle</h2>
            <p class="max-w-3xl mx-auto text-lg text-gray-600 text-center mb-12">
                O software é o que transforma o potencial do hardware em performance real. O domínio do controle em tempo real, especialmente o algoritmo PID, é o que separa os robôs rápidos dos robôs campeões. Experimente com o simulador para entender o impacto de cada parâmetro.
            </p>
            <div class="bg-white p-6 md:p-8 rounded-2xl shadow-xl">
                <h3 class="text-2xl font-bold mb-2">Simulador de Tuning PD</h3>
                <p class="text-gray-600 mb-6">A maioria dos robôs de alta velocidade usa um controle Proporcional-Derivativo (PD). Ajuste os ganhos Kp e Kd para ver como a resposta do robô muda. O objetivo é encontrar um equilíbrio que minimize o erro rapidamente, sem oscilações.</p>
                <div class="grid md:grid-cols-2 gap-8 items-center">
                    <div class="space-y-4">
                        <div>
                            <label for="kp-slider" class="block mb-2 font-medium">Ganho Proporcional (Kp): <span id="kp-value">5</span></label>
                            <input id="kp-slider" type="range" min="0" max="20" value="5" step="0.5" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer">
                            <p class="text-sm text-gray-500 mt-1">Reage ao erro atual. Alto demais causa oscilação.</p>
                        </div>
                        <div>
                            <label for="kd-slider" class="block mb-2 font-medium">Ganho Derivativo (Kd): <span id="kd-value">10</span></label>
                            <input id="kd-slider" type="range" min="0" max="50" value="10" step="1" class="w-full h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer">
                            <p class="text-sm text-gray-500 mt-1">Amortece a oscilação e suaviza a resposta.</p>
                        </div>
                    </div>
                    <div class="chart-container h-64 md:h-72">
                        <canvas id="pidChart"></canvas>
                    </div>
                </div>
            </div>
        </section>

        <section id="projects">
            <h2 class="text-3xl md:text-4xl font-bold text-center mb-4">Projetos Open-Source de Referência</h2>
            <p class="max-w-3xl mx-auto text-lg text-gray-600 text-center mb-12">
                Acelere seu desenvolvimento construindo sobre o trabalho da comunidade. Esta é uma seleção curada de projetos de código aberto testados em competição. Use-os como base para seu robô, focando seus esforços em inovar em vez de reinventar o básico.
            </p>
            <div class="flex justify-center flex-wrap gap-2 mb-8">
                <button class="filter-btn active px-4 py-2 bg-[#4DB6AC] text-white rounded-lg font-semibold" data-filter="all">Todos</button>
                <button class="filter-btn px-4 py-2 bg-gray-200 rounded-lg font-semibold" data-filter="stm32">STM32</button>
                <button class="filter-btn px-4 py-2 bg-gray-200 rounded-lg font-semibold" data-filter="esp32">ESP32/Arduino</button>
                <button class="filter-btn px-4 py-2 bg-gray-200 rounded-lg font-semibold" data-filter="vision">Visão</button>
            </div>
            <div id="projects-grid" class="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
            </div>
        </section>

    </main>
    
    <footer class="text-center py-6 mt-12 border-t border-gray-200">
        <p class="text-gray-500">Aplicação Interativa gerada a partir de relatório técnico sobre robótica de competição.</p>
    </footer>

    <script>
        const architectureData = {
            1: {
                title: "Array IR (Digital) + STM32/ESP32",
                pros: "Latência extremamente baixa; alta frequência de loop de controle; robustez a ruído elétrico.",
                cons: "Menor resolução posicional por leitura; requer interpolação de software para suavidade.",
                ideal: "Velocidade pura em pistas bem definidas; a escolha padrão para a maioria das competições de alto nível.",
                scores: { latency: 9, complexity: 5, cost: 6 }
            },
            2: {
                title: "Array IR (Analógico) + STM32/ESP32",
                pros: "Resolução posicional muito alta (média ponderada); controle inerentemente suave.",
                cons: "Suscetível a ruído elétrico; tempo de conversão ADC pode limitar a frequência do loop.",
                ideal: "Competições que exigem extrema suavidade e precisão, mesmo em detrimento da velocidade máxima.",
                scores: { latency: 7, complexity: 6, cost: 7 }
            },
            3: {
                title: "Câmera + Raspberry Pi (ROS/OpenCV)",
                pros: "Riqueza de dados; capacidade de lidar com pistas complexas (intersecções, gaps); robustez à iluminação.",
                cons: "Alta latência; alto consumo de energia; alta complexidade de software (Linux, ROS, OpenCV).",
                ideal: "Competições \"Enhanced\" com obstáculos e pistas complexas; projetos de P&D.",
                scores: { latency: 3, complexity: 9, cost: 8 }
            },
            4: {
                title: "Câmera + FPGA",
                pros: "Latência mínima absoluta; processamento de imagem massivamente paralelo em hardware.",
                cons: "Complexidade de desenvolvimento extrema (requer HDL); custo muito elevado; curva de aprendizado íngreme.",
                ideal: "Aplicações de ponta, pesquisa acadêmica avançada, ou onde cada nanossegundo conta.",
                scores: { latency: 10, complexity: 10, cost: 10 }
            }
        };

        const projectsData = [
            {
                name: "STM32 Line Follower with PID",
                platform: "stm32",
                mcu: "STM32F103C8",
                sensor: "QTR-8RC (analógico)",
                description: "Excelente documentação e base de código PID robusta. Ponto de partida ideal para a plataforma STM32.",
                link: "https://github.com/sametoguten/STM32-Line-Follower-with-PID"
            },
            {
                name: "LineFollower GRUZIK2.0-light",
                platform: "stm32",
                mcu: "STM32F103C8",
                sensor: "8x KTIR0711S (digital)",
                description: "Design ultraleve (62g), drivers eficientes e Bluetooth para tuning remoto. Otimizado para competição.",
                link: "https://github.com/NYDEREK/LineFollower_GRUZIK2.0-light"
            },
            {
                name: "ESP-32 Line Follower",
                platform: "esp32",
                mcu: "ESP32",
                sensor: "Array de 8 sensores IR",
                description: "Bom exemplo de implementação PID em ESP32, combinando poder de processamento com a facilidade do ecossistema Arduino.",
                link: "https://github.com/amjed-ali-k/ESP-32-Line-Follower"
            },
            {
                name: "PID Line Follower (Arduino)",
                platform: "esp32",
                mcu: "Arduino (Uno/Nano)",
                sensor: "Array de 5 sensores IR",
                description: "Projeto clássico e ideal para aprender os fundamentos do controle PID em um ambiente simples e acessível.",
                link: "https://github.com/vinamrsachdeva/PID-Line-Follower"
            },
            {
                name: "ROS Line Follower Robot",
                platform: "vision",
                mcu: "Raspberry Pi + Arduino",
                sensor: "Câmera",
                description: "Blueprint completo para um robô baseado em visão. Demonstra a arquitetura de sistema com RPi e ROS.",
                link: "https://github.com/chrisdalke/ros-line-follower-robot"
            }
        ];

        let architectureChart, mcuChart, pidChart;
        
        function updateArchitectureDetails(id) {
            const data = architectureData[id];
            const detailsContainer = document.getElementById('architecture-details');
            detailsContainer.innerHTML = `
                <h3 class="text-2xl font-bold mb-3">${data.title}</h3>
                <div class="space-y-3">
                    <div>
                        <h4 class="font-semibold text-green-600">Prós:</h4>
                        <p class="text-gray-600">${data.pros}</p>
                    </div>
                    <div>
                        <h4 class="font-semibold text-red-600">Contras:</h4>
                        <p class="text-gray-600">${data.cons}</p>
                    </div>
                    <div>
                        <h4 class="font-semibold text-[#4DB6AC]">Ideal Para:</h4>
                        <p class="text-gray-600">${data.ideal}</p>
                    </div>
                </div>
            `;
            updateArchitectureChart(id);
        }

        function selectArchitecture(id) {
            document.querySelectorAll('.architecture-card').forEach(card => card.classList.remove('selected'));
            document.getElementById(`arch-card-${id}`).classList.add('selected');
            updateArchitectureDetails(id);
        }

        function updateArchitectureChart(id) {
            const data = architectureData[id].scores;
            architectureChart.data.datasets[0].data = [data.latency, data.complexity, data.cost];
            architectureChart.update();
        }

        function createArchitectureChart() {
            const ctx = document.getElementById('architectureChart').getContext('2d');
            architectureChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Performance (Latência Inversa)', 'Complexidade', 'Custo'],
                    datasets: [{
                        label: 'Pontuação da Arquitetura',
                        data: [],
                        backgroundColor: ['#4DB6AC', '#F4A261', '#E76F51'],
                        borderRadius: 5,
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    indexAxis: 'y',
                    scales: {
                        x: {
                            beginAtZero: true,
                            max: 10,
                            grid: { display: false }
                        },
                        y: {
                            grid: { display: false }
                        }
                    },
                    plugins: {
                        legend: { display: false },
                        title: {
                            display: true,
                            text: 'Análise Comparativa da Arquitetura (Escala 1-10)',
                            font: { size: 14 }
                        }
                    }
                }
            });
        }

        function createMcuChart() {
            const ctx = document.getElementById('mcuChart').getContext('2d');
            mcuChart = new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: ['Poder de Processamento', 'Facilidade de Uso', 'Custo-Benefício', 'Capacidade de I/O', 'Ecossistema'],
                    datasets: [{
                        label: 'Arduino',
                        data: [2, 9, 8, 4, 10],
                        borderColor: '#E76F51',
                        backgroundColor: 'rgba(231, 111, 81, 0.2)',
                    }, {
                        label: 'ESP32',
                        data: [7, 8, 9, 7, 9],
                        borderColor: '#F4A261',
                        backgroundColor: 'rgba(244, 162, 97, 0.2)',
                    }, {
                        label: 'STM32',
                        data: [9, 5, 7, 9, 6],
                        borderColor: '#4DB6AC',
                        backgroundColor: 'rgba(77, 182, 172, 0.2)',
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        title: {
                            display: true,
                            text: 'Comparativo de Microcontroladores'
                        }
                    }
                }
            });
        }

        function createPidChart() {
            const ctx = document.getElementById('pidChart').getContext('2d');
            pidChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: Array.from({length: 100}, (_, i) => i),
                    datasets: [{
                        label: 'Resposta do Robô',
                        data: [],
                        borderColor: '#4DB6AC',
                        borderWidth: 2,
                        pointRadius: 0,
                        tension: 0.4
                    }, {
                        label: 'Setpoint (Linha)',
                        data: Array(100).fill(0),
                        borderColor: '#2c3e50',
                        borderWidth: 2,
                        borderDash: [5, 5],
                        pointRadius: 0,
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    animation: { duration: 0 },
                    scales: {
                        y: {
                            min: -10,
                            max: 10,
                            title: { display: true, text: 'Erro' }
                        },
                        x: {
                            display: false
                        }
                    },
                    plugins: {
                        legend: { display: false }
                    }
                }
            });
        }

        function updatePidChart() {
            const kp = parseFloat(document.getElementById('kp-slider').value);
            const kd = parseFloat(document.getElementById('kd-slider').value);
            document.getElementById('kp-value').textContent = kp;
            document.getElementById('kd-value').textContent = kd;

            let error = 5;
            let lastError = 5;
            let integral = 0;
            const responseData = [];
            let velocity = 0;

            for(let i = 0; i < 100; i++) {
                responseData.push(error);
                let proportional = kp * error;
                let derivative = kd * (error - lastError);
                let correction = proportional + derivative;
                
                velocity = (velocity * 0.5) - (correction * 0.1);
                error += velocity;

                lastError = responseData[responseData.length - 1];
            }
            pidChart.data.datasets[0].data = responseData;
            pidChart.update();
        }

        function openTab(evt, tabName) {
            let i, tabcontent, tablinks;
            tabcontent = document.getElementsByClassName("tab-content");
            for (i = 0; i < tabcontent.length; i++) {
                tabcontent[i].style.display = "none";
            }
            tablinks = document.getElementsByClassName("tab-button");
            for (i = 0; i < tablinks.length; i++) {
                tablinks[i].className = tablinks[i].className.replace(" active", "");
            }
            document.getElementById(tabName).style.display = "block";
            evt.currentTarget.className += " active";
        }

        function renderProjects(filter = 'all') {
            const grid = document.getElementById('projects-grid');
            grid.innerHTML = '';
            const filteredProjects = filter === 'all' ? projectsData : projectsData.filter(p => p.platform === filter);
            
            filteredProjects.forEach(project => {
                const card = `
                    <div class="card project-card bg-white rounded-xl shadow-md overflow-hidden" data-platform="${project.platform}">
                        <div class="p-6">
                            <h4 class="font-bold text-xl mb-1">${project.name}</h4>
                            <div class="flex flex-wrap gap-x-2 gap-y-1 text-xs mb-3">
                                <span class="bg-blue-100 text-blue-800 px-2 py-0.5 rounded-full">${project.mcu}</span>
                                <span class="bg-green-100 text-green-800 px-2 py-0.5 rounded-full">${project.sensor}</span>
                            </div>
                            <p class="text-gray-600 mb-4">${project.description}</p>
                            <a href="${project.link}" target="_blank" rel="noopener noreferrer" class="font-semibold text-[#4DB6AC] hover:text-[#3a8f86] transition-colors">Ver no GitHub &rarr;</a>
                        </div>
                    </div>
                `;
                grid.innerHTML += card;
            });
        }

        document.addEventListener('DOMContentLoaded', () => {
            createArchitectureChart();
            createMcuChart();
            createPidChart();

            selectArchitecture(1);
            updatePidChart();
            renderProjects();

            document.getElementById('kp-slider').addEventListener('input', updatePidChart);
            document.getElementById('kd-slider').addEventListener('input', updatePidChart);

            document.querySelectorAll('.filter-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    document.querySelectorAll('.filter-btn').forEach(btn => {
                        btn.classList.remove('active', 'bg-[#4DB6AC]', 'text-white');
                        btn.classList.add('bg-gray-200');
                    });
                    e.target.classList.add('active', 'bg-[#4DB6AC]', 'text-white');
                    e.target.classList.remove('bg-gray-200');
                    renderProjects(e.target.dataset.filter);
                });
            });
            
            document.getElementById('mobile-nav').addEventListener('change', function() {
                window.location.hash = this.value;
            });
            
            const navLinks = document.querySelectorAll('.nav-link');
            const sections = document.querySelectorAll('section');

            window.addEventListener('scroll', () => {
                let current = '';
                sections.forEach(section => {
                    const sectionTop = section.offsetTop;
                    if (pageYOffset >= sectionTop - 100) {
                        current = section.getAttribute('id');
                    }
                });

                navLinks.forEach(link => {
                    link.classList.remove('active');
                    if (link.getAttribute('href').includes(current)) {
                        link.classList.add('active');
                    }
                });
                
                const mobileNav = document.getElementById('mobile-nav');
                if (mobileNav.value !== `#${current}`) {
                    mobileNav.value = `#${current}`;
                }
            });
        });
    </script>
</body>
</html>
