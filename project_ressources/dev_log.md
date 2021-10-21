# Projet 3 : Ohmyfood

## Index

- Day #1
- Day #2

## Day #1

https://www.webdesignerdepot.com/2020/12/2-smartest-ways-to-structure-sass/

    sass/
    |
    |- abstracts/
    |  |- _mixins             // Sass Mixins Folder
    |  |- _variables.scss     // Sass Variables
    |
    |- core/
    |  |- _reset.scss         // Reset
    |  |- _typography.scss    // Typography Rules
    |
    |- components/
    |  |- _buttons.scss       // Buttons
    |  |- _carousel.scss      // Carousel
    |  |- _slider.scss        // Slider
    |
    |- layout/
    |  |- _navigation.scss    // Navigation
    |  |- _header.scss        // Header
    |  |- _footer.scss        // Footer
    |  |- _sidebar.scss       // Sidebar
    |  |- _grid.scss          // Grid
    |
    |- pages/
    |  |- _home.scss          // Home styles
    |  |- _about.scss         // About styles
    |
    |- sections/ (or blocks/)
    |  |- _hero.scss          // Hero section
    |  |- _cta.scss           // CTA section
    |
    |- vendors/ (if needed)
    |  |- _bootstrap.scss     // Bootstrap
    |
    - app.scss                // Main Sass file