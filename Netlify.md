# Deploy to Netlifly
## <span style="color:blue">Git config</span>
1. Primeiro passo é criar um repositório no Github.
2. Depois fazer um push do repositório local para o repositório do Github.
3. Adicionar projeto na página do Netlify.
4. Verificar se Git LFS está instalado, para usar arquivos que não são textos com o Github.
5. Instalar Netlify CLI globalmente.
6. Vincular o projeto local com o projeto do Netlify `` netlify link ``.
7. Instalar suporte para Large Media ``` netlify lm:setup ``` dentro da pasta do projeto.
8. <i class="fab fa-gitlab fa-fw" style="color:rgb(107,79,187); font-size:.85em" aria-hidden="true"></i> Configurar arquivos que serão monitorados.
	1. ``` git lfs track "dog.jpg" "cat.gif" ```
	2. ``` git lfs track "static/pdfs/**" ```
	3. ``` git lfs track "*.jpg" "*.png" ```
9. Estes comando geram um aquivo de configuração ``` .gitattributes ``` 
10. Para ver os tipos de arquivos que estão sendo monitorados ``` git lfs track ```
11.  Para ver os arquivos que estão sendo monitorados ``` git lfs ls-files ```

https://docs.netlify.com/large-media/overview/?_ga=2.249649795.429133245.1657834739-639604781.1657834739
 