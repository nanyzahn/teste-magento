Desafio - Processo seletivo
---------------------------------------------------------------


<h2>Via Composer</h2>
<p>Para essa instalação é necessário ter instalado o <a href="https://getcomposer.org" rel="nofollow">Composer</a> você precisa usar o terminal do seu servidor.</p>

<h2>1º Passo - Baixar o Magento</h2>

Comando usado:
<pre>
<code>composer create-project --repository=https://repo.magento.com/ magento/project-community-edition:2.3.5 m235</code></pre>

<h2>2º Passo - Solucionar dois problemas <p>(pois um deles afeta na instalação).</p></h2>

<p>ERRO: Unable to apply data patch Magento\Theme\Setup\Patch\Data\RegisterThemes for module Magento_Theme. Original exception message: Wrong file</p>

<p>Solução:
</p>
<p>Find validateURLScheme function in vendor\magento\framework\Image\Adapter\Gd2.php file. at line 96. Replace function with this:</p>

<code>private function validateURLScheme(string $filename) : bool
  {
      $allowed_schemes = ['ftp', 'ftps', 'http', 'https'];
      $url = parse_url($filename);
      if ($url && isset($url['scheme']) && !in_array($url['scheme'], $allowed_schemes) && !file_exists($filename)) {
          return false;
      }

      return true;
  }
</code>
<h3>Problema de barras '/' no Windows</h3>

<p>Problem: 1 exception(s):
Exception #0 (Magento\Framework\Exception\ValidatorException): Invalid template file: 'C:/laragon/www/teste/vendor/magento/module-theme/view/frontend/templates/page/js/require_js.phtml' in module: '' block's name: 'require.js'
</p>
<p>Solução:
</p>
<p>or Windows, this is the workaround for now, modify this below file {Magento_Dir}\vendor\magento\framework\View\Element\Template\File\Validator.php

Comment the existing $realpath around line 138 and add the new $realPath</p>

<code>//$realPath = $this->fileDriver->getRealPath($path);
$realPath = str_replace('\\', '/', $this->fileDriver->getRealPath($path));
</code>

<h2>3º Passo - Instalação </h2>

Comando utilizado:
<pre>
$ <code>bin/magento setup:install --backend-frontname=admin --db-name=teste --db-user=root --db-password="" --db-host=127.0.0.1 --admin-user=admin --admin-password=Senha --admin-email="nanyzahn@gmail.com" --admin-firstname=Nayara --admin-lastname=Zahn --currency=BRL --session-save=files </code>
</pre>

<b>Após finalizar a instalação, usar os comandos abaixo:</b>

<pre>
<code>
php bin/magento indexer:reindex
php bin/magento setup:upgrade
php bin/magento setup:static-content:deploy -f
php bin/magento cache:flush
</code>
</pre>


<h2>4º Passo - Instalação do Sample Data </h2>

<p>Comandos:</p>

<pre>
<code>
php bin/magento sampledata:deploy
php bin/magento module:enable --all
php bin/magento setup:upgrade
php bin/magento cache:flush
php bin/magento cache:clean
</code>
</pre>
----
<p>Fonte: https://mirasvit.com/knowledge-base/magento-2-installation-guide-composer-sample-data.html</p>

<h2>5º Passo - Setando Modo desenvolvedor</h2>

<pre>
<code>
$ bin/magento deploy:mode:set developer
Enabled developer mode.
</code>
</pre>

<h2>6º Passo - Traduzindo o Magento para PT-BR </h2>

<pre><code>composer require rafaelstz/traducao_magento2_pt_br:dev-master
php bin/magento setup:static-content:deploy pt_BR -f
php bin/magento cache:clean
</code></pre>

<h2> ou manualmente </h2>
<ul>
<li>Crie o diretório <strong>app/i18n/rafaelcg/language_pt_br</strong></li>
<li>Efetue o <a href="https://github.com/rafaelstz/traducao_magento2_pt_br/archive/master.zip">download do zip</a></li>
<li>Mova o conteúdo do repositório para a pasta e habilite a tradução</li>
</ul>


-----------------------------------------------
<h2>Subindo magento no Github</h2>
-----------------------------------------------
<pre>
$<code>git add .
$ git commit -m "Adicionado Magento 2.3.5"
$ git remote add origin https://github.com/nanyzahn/teste-magento.git
$ git push -u origin master </code></pre>
----------------

<h2>7º Passo - Criar uma Branch para adicionar as novas funcionalidades </h2>

<pre><code>git checkout -b dental
git status
git push --set-upstream origin dental
</code></pre>

