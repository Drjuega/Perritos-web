<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>¡Perritos Tiernos!</title>
    <!-- Tailwind CSS for modern styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts for friendly typography -->
    <link href="https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Quicksand', sans-serif;
            touch-action: manipulation;
        }
        @keyframes floatHeart {
            0% { transform: translateY(0px) scale(0.8); opacity: 1; }
            100% { transform: translateY(-120px) scale(1.4); opacity: 0; }
        }
        .floating-heart {
            position: absolute;
            animation: floatHeart 1.5s ease-out forwards;
            pointer-events: none;
        }
        .pulse-gentle {
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
    </style>
</head>
<body class="bg-gradient-to-br from-pink-100 via-purple-50 to-amber-50 min-h-screen flex flex-col justify-between items-center text-gray-800 overflow-hidden select-none">

    <header class="w-full pt-8 pb-4 text-center z-10">
        <h1 id="main-title" class="text-3xl font-bold text-pink-600 drop-shadow-sm transition-all duration-300">
            🐾 ¡Hola, amig@! 🐾
        </h1>
        <p id="sub-title" class="text-sm text-gray-600 mt-1 px-4">
            Toca la pantalla para descubrir algo maravilloso
        </p>
    </header>

    <main id="app-container" class="w-full flex-1 flex flex-col items-center justify-center px-6 relative cursor-pointer" onclick="handleCardClick(event)">
        
        <!-- Interactive Card Wrapper -->
        <div id="card-wrapper" class="relative w-full max-w-xs bg-white rounded-3xl shadow-xl p-4 transition-all duration-500 transform hover:scale-105 border-4 border-pink-200">
            
            <!-- Image container with loading placeholder fallback -->
            <div class="relative w-full h-80 rounded-2xl overflow-hidden bg-pink-50 flex items-center justify-center">
                <img id="puppy-image" 
                     src="https://images.unsplash.com/photo-1543466835-00a7907e9de1?auto=format&fit=crop&w=600&q=80" 
                     onerror="this.src='https://placehold.co/600x800/ffccd5/fff?text=Perrito+Simpatico';"
                     alt="Perrito tierno" 
                     class="w-full h-full object-cover transition-opacity duration-500">
                
                <!-- Badge indicator -->
                <span id="puppy-badge" class="absolute top-3 left-3 bg-pink-500 text-white text-xs font-bold px-3 py-1 rounded-full shadow-md uppercase tracking-wider">
                    Paso 1 de 2
                </span>
            </div>

            <!-- Description / Call to action text -->
            <div class="mt-4 text-center">
                <p id="puppy-message" class="text-lg font-bold text-pink-700">
                    ¡Toca para ver un perrito más bonito! 🐶
                </p>
                <div class="mt-3 inline-flex items-center justify-center bg-pink-500 hover:bg-pink-600 active:scale-95 text-white font-semibold py-2 px-6 rounded-full shadow-lg transition-all text-sm w-full">
                    <span id="button-text">¡Toca aquí! ✨</span>
                </div>
            </div>
        </div>

    </main>

    <footer class="w-full pb-6 text-center z-10">
        <p class="text-xs text-gray-400">Diseñado con amor y perritos 💖</p>
    </footer>

    <script>
        // State variables
        let currentState = 1;

        // Data for the two puppy states
        const puppyData = {
            1: {
                title: "🐾 ¡Hola, amig@! 🐾",
                subtitle: "Toca la pantalla para descubrir algo maravilloso",
                badge: "Paso 1 de 2",
                image: "https://images.unsplash.com/photo-1543466835-00a7907e9de1?auto=format&fit=crop&w=600&q=80",
                message: "¡Toca para ver un perrito más bonito! 🐶",
                buttonText: "¡Toca aquí! ✨"
            },
            2: {
                title: "💖 ¡Misión Cumplida! 💖",
                subtitle: "¡Advertencia: Demasiada ternura en pantalla!",
                badge: "¡El más bonito! 😍",
                image: "https://images.unsplash.com/photo-1583511655857-d19b40a7a54e?auto=format&fit=crop&w=600&q=80",
                message: "¡Imposible resistirse a este angelito! 🥰",
                buttonText: "🔄 Volver a empezar"
            }
        };

        // Handle card touch/click interactions
        function handleCardClick(event) {
            // Prevent event bubbling issues
            event.stopPropagation();

            // Toggle state between 1 and 2
            currentState = currentState === 1 ? 2 : 1;

            // Trigger visual updates
            updateUI(currentState);

            // Spawn floating hearts at touch/click coordinates
            const clientX = event.clientX || (event.touches && event.touches[0].clientX) || window.innerWidth / 2;
            const clientY = event.clientY || (event.touches && event.touches[0].clientY) || window.innerHeight / 2;
            createFloatingHearts(clientX, clientY);
        }

        // Update DOM elements smoothly
        function updateUI(state) {
            const data = puppyData[state];
            
            const mainTitle = document.getElementById('main-title');
            const subTitle = document.getElementById('sub-title');
            const puppyImage = document.getElementById('puppy-image');
            const puppyBadge = document.getElementById('puppy-badge');
            const puppyMessage = document.getElementById('puppy-message');
            const buttonText = document.getElementById('button-text');
            const cardWrapper = document.getElementById('card-wrapper');

            // Fade out slightly
            cardWrapper.style.transform = 'scale(0.95)';
            puppyImage.style.opacity = '0.3';

            setTimeout(() => {
                mainTitle.textContent = data.title;
                subTitle.textContent = data.subtitle;
                puppyImage.src = data.image;
                puppyBadge.textContent = data.badge;
                puppyMessage.textContent = data.message;
                buttonText.textContent = data.buttonText;

                // Fade back in
                puppyImage.style.opacity = '1';
                cardWrapper.style.transform = 'scale(1)';
            }, 200);
        }

        // Generate floating heart emojis on click/tap
        function createFloatingHearts(x, y) {
            const emojis = ['💖', '🐶', '✨', '🐾', '🌸', '💫'];
            for (let i = 0; i < 6; i++) {
                const heart = document.createElement('div');
                heart.className = 'floating-heart text-2xl';
                heart.textContent = emojis[Math.floor(Math.random() * emojis.length)];
                
                // Random offset around touch point
                const offsetX = (Math.random() - 0.5) * 80;
                const offsetY = (Math.random() - 0.5) * 40;
                
                heart.style.left = `${x + offsetX}px`;
                heart.style.top = `${y + offsetY}px`;
                
                document.body.appendChild(heart);

                // Remove element after animation ends
                setTimeout(() => {
                    heart.remove();
                }, 1500);
            }
        }
    </script>
</body>
</html>
