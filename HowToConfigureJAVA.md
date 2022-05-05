# Configuring JAVA environment for neovim
- install java sdk
- download from https://download.eclipse.org/jdtls/snapshots/ the latest and
  unzip it to any folder that you'll use
- add to nvim config: 
    - plug 'prabirshrestha/vim-lsp' 
    - plug 'mattn/vim-lsp-settings'
    - Plug 'mfussengger/nvim-jdtls'
- check nvim-jdtls and create ftplugin folder with java.lua and replace needed
  urls
- call CocInstall coc-java 
    - call CocConfig and add there "java.home":"C:/Program Files/Java/jdk-18.0.1.1"
    (Pay attention that if you place some wrong settings additionaly you'll get
    loading server Errors)
    - in case of error like 'can't start server' 
        - clear coc config file from wrong settings
        - copy from downloaded by you Latest jdtls to
          coc/Extensions/data/server/...
