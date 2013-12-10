;;用f4查看文件及里面的函数定义
(global-set-key [(f4)] 'speedbar-get-focus)
;;安装cscope-el，cscope
sudo apt-get install  cscope-el

;;(require 'xcscope)
;;只有打开c和c++文件才会加载cscope
(add-hook 'c-mode-common-hook '(lambda() (require 'xcscope)))

改cscope-indexer脚本，将
cscope -b -i $LIST_FILE -f $DATABASE_FILE
替换为
cscope -q -b -i $LIST_FILE -f $DATABASE_FILE
Cscope可以通过创建反向索引加速查找

cscope-indexer －r