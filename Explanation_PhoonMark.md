# Explicação do Código do PhoonMark
## Explanation of the PhoonMark Code

Este arquivo detalha o funcionamento do PhoonMark. Todo o código — HTML, CSS e JavaScript — está contido em um único arquivo, `index.html`, criando uma Aplicação de Página Única (Single-Page Application).

This file details how PhoonMark works. All the code — HTML, CSS, and JavaScript — is contained within a single `index.html` file, creating a Single-Page Application.

O código-fonte principal está dentro da tag `<script>` no final do arquivo `index.html`.
The main source code is inside the `<script>` tag at the end of the `index.html` file.

---

## Como funciona | How it works

1.  **Gerenciamento de Dados com `localStorage` / Data Management with `localStorage`**
    O aplicativo salva todas as informações no navegador do usuário. Listas de bookmarks, sites salvos e sites recentes são armazenadas localmente, garantindo que os dados persistam mesmo que a página seja fechada.
    The application saves all information in the user's browser. Lists of bookmarks, saved sites, and recent sites are stored locally, ensuring the data persists even if the page is closed.

2.  **Navegação via `iframe` / Navigation via `iframe`**
    Quando um usuário insere uma URL, o site correspondente é carregado dentro de um elemento `<iframe>`. Isso cria um ambiente de "navegador dentro do navegador", permitindo a interação com a página carregada.
    When a user enters a URL, the corresponding site is loaded inside an `<iframe>` element. This creates a "browser within a browser" environment, allowing interaction with the loaded page.

3.  **Execução de Bookmarklets / Bookmarklet Execution**
    Se um bookmark salvo contiver código JavaScript (`javascript:...`), o aplicativo o injeta e executa no contexto do `iframe`. Isso permite que os scripts manipulem a página que está sendo exibida.
    If a saved bookmark contains JavaScript code (`javascript:...`), the application injects and runs it within the context of the `iframe`. This allows the scripts to manipulate the page currently being displayed.

4.  **Renderização Dinâmica da Interface / Dynamic UI Rendering**
    As listas de bookmarks, sites recentes e salvos não são fixas no HTML. Elas são geradas dinamicamente com JavaScript com base nos dados salvos no `localStorage`, e a tela é atualizada sempre que um item é adicionado, editado ou removido.
    The lists of bookmarks, recent sites, and saved sites are not hardcoded in the HTML. They are generated dynamically with JavaScript based on the data saved in `localStorage`, and the display is updated whenever an item is added, edited, or removed.

5.  **Gerenciamento de Estado (Telas) / State Management (Views)**
    A aplicação alterna entre duas telas principais: a página inicial (`landing-page`) e a visualização do navegador (`browser-view`). O JavaScript gerencia qual tela é exibida, proporcionando uma transição fluida sem recarregar a página.
    The application switches between two main views: the `landing-page` and the `browser-view`. JavaScript manages which view is displayed, providing a smooth transition without reloading the page.

---

## Código comentado | Commented code

```javascript
document.addEventListener('DOMContentLoaded', () => {
    // Função para selecionar elementos do DOM de forma mais curta
    // Helper function to select DOM elements more concisely
    const $ = (selector) => document.querySelector(selector);
    
    // Mapeamento de todos os elementos da interface para variáveis
    // Mapping all UI elements to variables
    const landingPage = $('#landing-page'), browserView = $('#browser-view');
    const urlInput = $('#url-input'), urlSubmitBtn = $('#url-submit-btn');
    const contentFrame = $('#content-frame'), loaderOverlay = $('#loader-overlay');
    const menuToggleBtn = $('#menu-toggle-btn'), menuPanel = $('#menu-panel'), menuOverlay = $('#menu-overlay');
    const bookmarksList = $('#bookmarks-list'), addBookmarkForm = $('#add-bookmark-form');
    // ... (outras variáveis de elementos)

    let savedSites = [], bookmarks = [], recents = [];

    // Exibe uma notificação temporária na parte inferior da tela
    // Displays a temporary notification at the bottom of the screen
    const showToast = (message) => {
        toast.textContent = message;
        toast.classList.add('show');
        setTimeout(() => toast.classList.remove('show'), 3000);
    };

    // Funções para salvar e carregar dados do localStorage
    // Functions to save and load data from localStorage
    const saveData = (key, data) => localStorage.setItem(key, JSON.stringify(data));
    const loadData = (key) => JSON.parse(localStorage.getItem(key) || '[]');
    
    // Função principal para executar código JavaScript dentro do iframe
    // Main function to execute JavaScript code inside the iframe
    const executeBookmarklet = (code) => {
        try {
            const frameWindow = contentFrame.contentWindow;
            const frameDoc = contentFrame.contentDocument || frameWindow.document;

            // Tenta executar o código diretamente, o que é mais rápido
            // Tries to execute the code directly, which is faster
            try {
                frameWindow.eval(code);
                return true;
            } catch (e1) {
                // Se falhar (devido a políticas de segurança), usa um método alternativo
                // If it fails (due to security policies), use an alternative method
                const script = frameDoc.createElement('script');
                script.textContent = code;
                frameDoc.head.appendChild(script).remove(); // Adiciona e remove o script
                return true;
            }
        } catch (e) {
            console.error("Erro de segurança ao tentar executar o bookmarklet:", e);
            return false;
        }
    };
    
    // Adiciona bookmarklets padrão se o usuário não tiver nenhum
    // Adds default bookmarklets if the user has none
    const addDefaultBookmarklets = () => {
        const defaultBookmarks = [
            { name: "UnBlockCopy - Desbloquear Cópia", url: "javascript:..." },
            { name: "View Password - Mostrar Senhas", url: "javascript:..." },
            { name: "Dark Mode - Modo Escuro", url: "javascript:..." }
        ];
        // ... (lógica para adicionar)
    };

    // Carrega todos os dados (sites, bookmarks, recentes) do localStorage
    // Loads all data (sites, bookmarks, recents) from localStorage
    const loadAllData = () => {
        savedSites = loadData('phoon_saved_sites');
        bookmarks = loadData('phoon_bookmarks');
        recents = loadData('phoon_recents');
        
        if (bookmarks.length === 0) {
            addDefaultBookmarklets();
        }
    };
    
    // Adiciona uma URL à lista de sites recentes
    // Adds a URL to the list of recent sites
    const addRecent = (url) => {
        // ... (lógica para gerenciar a lista de recentes)
    };

    // Função genérica para renderizar qualquer lista na interface
    // Generic function to render any list in the UI
    const renderList = (container, items, template, emptyMessage) => {
        // ... (cria o HTML para cada item e o insere no container)
    };

    // Chama a renderList para atualizar todas as listas na tela
    // Calls renderList to update all lists on the screen
    const renderAllLists = () => {
        renderList(recentSitesList, recents, /*...*/);
        renderList(savedSitesList, savedSites, /*...*/);
        renderList(bookmarksList, bookmarks, /*...*/);
    };

    // Atualiza o ícone de estrela (salvo/não salvo) com base na URL atual
    // Updates the star icon (saved/not saved) based on the current URL
    const updateStarIcon = () => {
        const currentUrl = contentFrame.src;
        const isSaved = savedSites.some(site => site.url === currentUrl);
        // ... (lógica para alterar a aparência da estrela)
    };

    // Navega para uma nova URL dentro do iframe
    // Navigates to a new URL inside the iframe
    const navigateTo = (url) => {
        // ... (lógica para formatar a URL e atribuí-la ao src do iframe)
    };

    // Alterna a visibilidade do painel de menu lateral
    // Toggles the visibility of the side menu panel
    const toggleMenu = (open) => {
        // ... (adiciona/remove classes CSS para mostrar/ocultar o menu)
    };
    
    // Inicia a navegação, trocando da tela inicial para a tela do navegador
    // Starts navigation, switching from the landing page to the browser view
    const startNavigation = (url) => {
        landingPage.style.display = 'none';
        browserView.classList.remove('hidden');
        browserView.classList.add('flex');
        navigateTo(url);
    };

    // Gerencia o envio da URL do input principal
    // Handles the URL submission from the main input
    const submitUrl = () => {
        const url = urlInput.value.trim();
        if (url) {
            startNavigation(url);
        }
    };

    // Adiciona os "escutadores de eventos" para interações do usuário
    // Adds event listeners for user interactions
    urlSubmitBtn.addEventListener('click', submitUrl);
    urlInput.addEventListener('keydown', (e) => { if (e.key === 'Enter') submitUrl(); });
    
    contentFrame.addEventListener('load', () => {
        // Quando o iframe termina de carregar, esconde o loader
        // When the iframe finishes loading, hide the loader overlay
         loaderOverlay.style.opacity = '0';
         setTimeout(() => { loaderOverlay.style.display = 'none'; }, 300);
         updateStarIcon();
    });
    
    menuToggleBtn.addEventListener('click', () => toggleMenu(true));
    homeBtn.addEventListener('click', goToHome);

    // Evento para salvar/desalvar um site (clique na estrela)
    // Event for saving/unsaving a site (click on the star)
    saveSiteStarBtn.addEventListener('click', () => { /*...*/ });

    // Evento para adicionar um novo bookmark a partir do formulário
    // Event for adding a new bookmark from the form
    addBookmarkForm.addEventListener('submit', (e) => { /*...*/ });

    // Gerencia cliques na lista de bookmarks (executar, editar, deletar)
    // Manages clicks on the bookmarks list (execute, edit, delete)
    bookmarksList.addEventListener('click', (e) => {
        const item = e.target.closest('[data-url]');
        // ... (lógica para diferenciar entre executar, editar e deletar)
        if (item) {
            const url = item.dataset.url;
            if (url.startsWith('javascript:')) {
                executeBookmarklet(/*...code...*/);
            } else {
                navigateTo(url);
            }
        }
    });

    // ... (outros eventos para abas, edição, etc.)

    // Inicia a aplicação carregando os dados e renderizando as listas
    // Initializes the application by loading data and rendering the lists
    loadAllData();
    renderAllLists();
});
