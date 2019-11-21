## github hooks 配置教程 ，使用钩子实现网站自动化部署。

##### 一. 在github上添加一个远程仓库。

##### 二. 在本地初始化一个仓库并关联github上创建的远程仓库。

`$ git remote add origin https://github.com/purexu/noteBook.git`

##### 三. 把本地库的所有内容推送到远程库上。

`$ git push -u origin master`

做完以上三步，你的本地仓库就和github上的远程仓库关联啦！

##### 四. 在linux中通过如下命令查看执行php程序的用户。

`$ ps -ef | grep php`

##### 五. 在linux系统中生成两个git密钥，一个配置在github的 *ssh key* 中，一个配置在项目的 *deploy key* 中。并配置git的全局名称和邮箱。命令如下：

`$ git config --global user.name "helloxu"`

`$ git config --global user.email "2287659301@qq.com"`

`$ ssh-keygen -t rsa -C "xxxx@qq.com"`

##### 六. 在服务器的网站运行目录下克隆一份github下要关联的项目。注意使用 php程序执行的用户克隆。

`$ git clone https://github.com/purexu/phpBlog.git`

##### 七. 在服务器的网站运行目录下创建一个 *deploy.php* 文件，该文件作为脚本配置在github中要用钩子的项目中，在该项目的 *web hooks* 中配置好脚本运行的地址和密码。脚本内容如下：

```  php
<?php
 
class Deployment {
 
	public $serect = 'mengling@1234333'; //webhooks中配置的密钥
 
	public function deploy()
	{
		$requestBody = file_get_contents('php://input'); //每次推送的时候，会接收到post过来的数据。
		$payload = json_decode($requestBody, true);    //将数据转成数组，方便取值。
		if(empty($payload)){
            //写日志
			$this->write_log('send fail from github is empty');exit;
		}else{
            //获取github推送代码时经过哈希加密密钥的值
			$signature = $_SERVER['HTTP_X_HUB_SIGNATURE'];
		}
		
		if (strlen($signature) > 8 && $this->isFromGithub($requestBody,$signature)) {
            //验证密钥是否正确，如果正确执行命令。
			$res = shell_exec("cd /www/wwwroot/phpBlog &&                         
git pull 2>&1");
			$res_log = "\n -------------------------".PHP_EOL;
			$res_log .= '['.$payload['commits'][0]['author']['name'] . ']' . '向[' . $payload['repository']['name'] . ']项目的' . $payload['ref'] . '分支'.$_SERVER['X-GitHub-Event'].'了代码。commit信息是：'.$payload['commits']['message'].'。详细信息如下：' . PHP_EOL;
			$res_log .= $res.PHP_EOL;
			http_response_code(200);
			$this->write_log($res_log);
		}else{
			$this->write_log('git 提交失败！');
			abort(403);
		}
	}
	
	public function isFromGithub($payload,$signature)
	{
        //$hash是github的密钥。然后与本地的密钥做对比。
		list($algo, $hash) = explode("=", $signature, 2); 
		return $hash === hash_hmac($algo, $payload, $this->serect);
	}
 
	public function write_log($data)
	{
		// 此处加载日志类，用来记录git push信息，可以自行写。
        file_put_contents(dirname(_FILE_)."/logs.txt",serialize($data));
	}
}
 
$deploy = new Deployment();
 
if($_SERVER['REQUEST_METHOD'] == 'POST'){
    //触发此代码的时候，git是以post方式触发
	$signature = $deploy->deploy();
}
```

##### 八. 如有疑惑请参考链接：https://blog.csdn.net/weixin_42661321/article/details/88857178