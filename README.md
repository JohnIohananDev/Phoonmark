# PhoonMark

Uma ferramenta criada para simplificar o uso de bookmarklets em sites, principalmente pelo celular.
A tool designed to simplify running bookmarklets on websites, especially on mobile.

**Site:** [Phoonmark](https://phoonmark.netlify.app)

---

## O que é? | What is it?

Usar bookmarklets em navegadores de celular pode ser complicado. PhoonMark resolve isso: ele oferece uma interface onde você cola o link de um site e, com um toque, executa seus scripts salvos, sem precisar instalar nada no navegador.

É uma solução prática para quem precisa rodar scripts de automação, depuração ou utilitários em páginas da web diretamente do celular ou tablet.

Using bookmarklets on mobile browsers can be tricky. PhoonMark solves this by providing an interface where you paste a website link and, with a single tap, run your saved scripts without needing to install anything in your browser.

It's a practical solution for anyone who needs to run automation, debugging, or utility scripts on web pages directly from a mobile phone or tablet.

## Aviso | Disclaimer

Esta é uma versão inicial e ainda em desenvolvimento. Muitos sites modernos possuem políticas de segurança (como `X-Frame-Options`) que os impedem de serem carregados dentro de um `iframe`. Por isso, **PhoonMark pode não funcionar em 100% dos sites**. A funcionalidade é mais garantida em páginas que permitem sua incorporação.

This is an early version and still under development. Many modern websites have security policies (like `X-Frame-Options`) that prevent them from being loaded inside an `iframe`. Because of this, **PhoonMark may not work on 100% of websites**. Functionality is more reliable on pages that allow embedding.

## Funcionalidades | Features

* **Navegação em Iframe:** Carregue um site digitando ou colando a URL.
    * **Iframe Navigation:** Load a website by typing or pasting its URL.
* **Gerenciador de Bookmarks:** Adicione, edite e remova seus próprios bookmarks e bookmarklets.
    * **Bookmark Manager:** Add, edit, and remove your own bookmarks and bookmarklets.
* **Execução de Scripts:** Execute bookmarklets (`javascript:...`) diretamente na página carregada.
    * **Script Execution:** Run bookmarklets (`javascript:...`) directly on the loaded page.
* **Histórico e Favoritos:** Salve sites para acesso rápido e veja uma lista de sites recentes.
    * **History and Favorites:** Save sites for quick access and view a list of recent sites.
* **Persistência Local:** Seus dados ficam salvos no seu navegador usando `localStorage`.
    * **Local Persistence:** Your data is stored in your browser using `localStorage`.

## Como Usar | How to Use

1.  **Cole uma URL:** Na página inicial, insira o endereço do site que deseja carregar.
    * **Paste a URL:** On the home page, enter the address of the site you want to load.
2.  **Abra o Menu:** Clique no botão de menu no canto inferior direito para ver seus bookmarks.
    * **Open the Menu:** Click the menu button in the bottom-right corner to see your bookmarks.
3.  **Execute um Bookmarklet:** Clique em um item da lista. Se for um bookmarklet (JS), o script será executado na página que você carregou.
    * **Run a Bookmarklet:** Click an item in the list. If it's a bookmarklet (JS), the script will run on the page you loaded.

## Tecnologias | Tech Stack

* HTML5
* Tailwind CSS
* JavaScript (Vanilla)
