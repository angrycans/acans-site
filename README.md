# blog_hugo

git submodule init
git submodule update


theme
cd themes 
git clone https://github.com/flysnow-org/maupassant-hugo

find maupassant-hugo
delete {{ partial "related" . }}
      {{ partial "footer" . }}  


new post
hugo new post/nodeMCU_setting.md


review 
hugo server -D


publish
sh ./deploy.sh

