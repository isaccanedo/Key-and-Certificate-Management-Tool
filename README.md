# Key-and-Certificate-Management-Tool
:lock: # keytool - Ferramenta de gerenciamento de chave e certificado
Gerencia um keystore (banco de dados) de chaves criptográficas, cadeias de certificados X.509 e certificados confiáveis.

Nota: A interface do comando keytool foi alterada no Java SE 6. Observe que os comandos definidos anteriormente ainda são suportados.

### Descrição 

keytool é um utilitário de gerenciamento de chaves e certificados. Ele permite que os usuários administrem seus próprios pares de chaves públicas / privadas e certificados associados para uso em auto-autenticação (onde o usuário se autentica para outros usuários / serviços) ou integridade de dados e serviços de autenticação, usando assinaturas digitais. Ele também permite que os usuários armazenem em cache as chaves públicas (na forma de certificados) de seus pares em comunicação. Um certificado é uma declaração assinada digitalmente de uma entidade (pessoa, empresa, etc.), dizendo que a chave pública (e algumas outras informações) de alguma outra entidade tem um valor específico. (Consulte Certificados.) Quando os dados são assinados digitalmente, a assinatura pode ser verificada para verificar a integridade e autenticidade dos dados. Integridade significa que os dados não foram modificados ou adulterados e autenticidade significa que os dados realmente vêm de quem afirma tê-los criado e assinado.

O keytool também permite que os usuários administrem as chaves secretas usadas na criptografia / descriptografia simétrica (por exemplo, DES).

keytool armazena as chaves e certificados em um armazenamento de chaves.

### Comando e notas de opção
Os vários comandos e suas opções são listados e descritos a seguir. Observação:

- Todos os nomes de comandos e opções são precedidos por um sinal de menos (-).
- As opções para cada comando podem ser fornecidas em qualquer ordem.
- Todos os itens não em itálico ou entre colchetes ou colchetes devem aparecer como estão.
- Os colchetes ao redor de uma opção geralmente significam que um valor padrão será usado se a opção não for especificada na linha de comando. Chaves também são usadas em torno das opções -v, -rfc e -J, que só têm significado se aparecerem na linha de comando (ou seja, não têm nenhum valor "padrão" diferente de não existente).
- Os colchetes ao redor de uma opção significam que o usuário é solicitado a fornecer o (s) valor (es) se a opção não for especificada na linha de comando. (Para uma opção -keypass, se você não especificar a opção na linha de comando, o keytool tentará primeiro usar a senha do keystore para recuperar a chave privada / secreta e, se isso falhar, solicitará a chave privada / secreta senha da chave.)
- Os itens em itálico (valores de opção) representam os valores reais que devem ser fornecidos. Por exemplo, aqui está o formato do comando -printcert:

```
keytool -printcert {-file cert_file} {-v}
```

Ao especificar um comando -printcert, substitua cert_file pelo nome do arquivo real, como em:

```
keytool -printcert -file VScert.cer
```
- Os valores das opções devem ser colocados entre aspas se contiverem um espaço em branco.
- O comando -help é o padrão. Assim, a linha de comando

```
keytool
```
é equivalente a

```
keytool -help
```

### Opções padrão
Abaixo estão os padrões para vários valores de opção

```
-alias "mykey"

-keyalg
    "DSA" (when using -genkeypair)
    "DES" (when using -genseckey)

-keysize
    1024 (when using -genkeypair)
    56 (when using -genseckey and -keyalg is "DES")
    168 (when using -genseckey and -keyalg is "DESede")

-validity 90

-keystore the file named .keystore in the user's home directory

-storetype the value of the "keystore.type" property in the security properties file,
           which is returned by the static getDefaultType method in java.security.KeyStore

-file stdin if reading, stdout if writing

-protected false
```

Ao gerar um par de chaves pública / privada, o algoritmo de assinatura (opção -sigalg) é derivado do algoritmo da chave privada subjacente: Se a chave privada subjacente for do tipo "DSA", a opção -sigalg é padronizada para "SHA1withDSA", e se a chave privada subjacente for do tipo "RSA", o padrão -sigalg é "SHA256withRSA". Consulte a Especificação e referência da API da arquitetura de criptografia Java para obter uma lista completa de -keyalg e -sigalg à sua escolha.

### Opções Comuns
A opção -v pode aparecer para todos os comandos, exceto -help. Se aparecer, significa o modo "detalhado"; mais informações serão geradas.
Também existe uma opção -Jjavaoption que pode aparecer para qualquer comando. Se aparecer, a string javaoption especificada é passada diretamente para o interpretador Java. Esta opção não deve conter espaços. É útil para ajustar o ambiente de execução ou uso de memória. Para obter uma lista das opções possíveis do interpretador, digite java -h ou java -X na linha de comando.

Essas opções podem aparecer para todos os comandos que operam em um armazenamento de chaves:
```
-storetype storetype
```
Este qualificador especifica o tipo de keystore a ser instanciado.

```
-keystore keystore
```
#### O local do keystore
Se o tipo de armazenamento JKS for usado e um arquivo de armazenamento de chave ainda não existir, determinados comandos de keytool podem resultar na criação de um novo arquivo de armazenamento de chave. Por exemplo, se keytool -genkeypair for chamado e a opção -keystore não for especificada, o arquivo keystore padrão denominado .keystore no diretório inicial do usuário será criado se ainda não existir. Da mesma forma, se a opção -keystore ks_file for especificada, mas ks_file não existir, ele será criado.

Observe que o fluxo de entrada da opção -keystore é passado para o método KeyStore.load. Se NONE for especificado como o URL, um fluxo nulo será passado para o método KeyStore.load. NONE deve ser especificado se o KeyStore não for baseado em arquivo (por exemplo, se ele residir em um dispositivo de token de hardware).
```
-storepass storepass
```
A senha que é usada para proteger a integridade do armazenamento de chaves.
storepass deve ter pelo menos 6 caracteres. Deve ser fornecido a todos os comandos que acessam o conteúdo do keystore. Para tais comandos, se uma opção -storepass não for fornecida na linha de comandos, o usuário será solicitado a fazê-lo.

Ao recuperar informações do armazenamento de chaves, a senha é opcional; se nenhuma senha for fornecida, a integridade das informações recuperadas não poderá ser verificada e um aviso será exibido.

```
-providerName provider_name
Usado para identificar o nome de um provedor de serviços criptográficos quando listado no arquivo 
de propriedades de segurança.
```

```
-providerClass provider_class_name
Usado para especificar o nome do arquivo de classe principal do provedor de serviços criptográficos 
quando o provedor de serviços não está listado no arquivo de propriedades de segurança.
```

```
-providerArg provider_arg
Usado em conjunto com -providerClass. Representa um argumento de entrada de string opcional para o 
construtor de provider_class_name.
```
```
-protected
Ou true ou false. Este valor deve ser especificado como verdadeiro se uma senha deve ser fornecida 
por meio de um caminho de autenticação protegido, como um leitor de PIN dedicado.
```

### Comandos
#### Criação ou adição de dados ao armazenamento de chaves

```
-genkeypair {-alias alias} {-keyalg keyalg} {-keysize keysize} {-sigalg sigalg} [-dname dname] 
[-keypass keypass] {-validity valDays} {-storetype storetype} {-keystore keystore} 
[-storepass storepass] {-providerClass provider_class_name {-providerArg provider_arg}} {-v} 
{-protected} {-Jjavaoption}
```

Gera um par de chaves (uma chave pública e uma chave privada associada). Envolve a chave pública em um certificado autoassinado X.509 v3, que é armazenado como uma cadeia de certificados de elemento único. Esta cadeia de certificados e a chave privada são armazenadas em uma nova entrada de armazenamento de chaves identificada por alias.

keyalg especifica o algoritmo a ser usado para gerar o par de chaves e keysize especifica o tamanho de cada chave a ser gerada. sigalg especifica o algoritmo que deve ser usado para assinar o certificado autoassinado; este algoritmo deve ser compatível com keyalg.

dname especifica o Nome distinto X.500 a ser associado ao alias e é usado como o emissor e os campos de assunto no certificado autoassinado. Se nenhum nome distinto for fornecido na linha de comando, o usuário será solicitado a fornecer um.

keypass é uma senha usada para proteger a chave privada do par de chaves gerado. Se nenhuma senha for fornecida, o usuário será solicitado a inseri-la. Se você pressionar RETURN no prompt, a senha da chave será definida com a mesma senha usada para o armazenamento de chaves. o keypass deve ter pelo menos 6 caracteres.

valDays informa o número de dias durante os quais o certificado deve ser considerado válido.

Este comando era denominado -genkey em versões anteriores. Este nome antigo ainda é suportado neste lançamento e será suportado em lançamentos futuros, mas para esclarecer o novo nome, -genkeypair, é preferível daqui para frente.

```
-genseckey {-alias alias} {-keyalg keyalg} {-keysize keysize} [-keypass keypass] 
{-storetype storetype} {-keystore keystore} [-storepass storepass] 
{-providerClass provider_class_name {-providerArg provider_arg}} {-v} {-protected} 
{-Jjavaoption} Generates a secret key and stores it in a new 
KeyStore.SecretKeyEntry identified by alias.
```
"keyalg especifica o algoritmo a ser usado para gerar a chave secreta e keysize especifica o tamanho da chave a ser gerada. keypass é uma senha usada para proteger a chave secreta. Se nenhuma senha for fornecida, o usuário será solicitado a inseri-la. Se você pressionar RETURN no prompt, a senha da chave será definida com a mesma senha usada para o armazenamento de chaves. o keypass deve ter pelo menos 6 caracteres."

```
-importcert {-alias alias} {-file cert_file} [-keypass keypass] {-noprompt} 
{-trustcacerts} {-storetype storetype} {-keystore keystore} [-storepass storepass] 
{-providerName provider_name} {-providerClass provider_class_name {-providerArg provider_arg}} 
{-v} {-protected} {-Jjavaoption}
```

Lê o certificado ou a cadeia de certificados (onde o último é fornecido em uma resposta formatada em PKCS # 7) do arquivo cert_file e o armazena na entrada do keystore identificada pelo alias. Se nenhum arquivo for fornecido, o certificado ou a resposta PKCS # 7 é lida do stdin.

O keytool pode importar certificados X.509 v1, v2 e v3 e cadeias de certificados formatados em PKCS # 7 que consistem em certificados desse tipo. Os dados a serem importados devem ser fornecidos em formato de codificação binária ou em formato de codificação para impressão (também conhecido como codificação Base64), conforme definido pelo padrão Internet RFC 1421. No último caso, a codificação deve ser limitada no início por uma string que começa com "----- BEGIN" e no final por uma string que começa com "----- END".

Você importa um certificado por dois motivos:

1 - para adicioná-lo à lista de certificados confiáveis, ou
2 - para importar uma resposta de certificado recebida de uma CA como resultado do envio de uma Solicitação de Assinatura de Certificado (consulte o comando -certreq) para essa CA.

O tipo de importação pretendido é indicado pelo valor da opção -alias:

1 - Se o alias não apontar para uma entrada de chave, o keytool presume que você está adicionando uma entrada de certificado confiável. Nesse caso, o alias ainda não deve existir no armazenamento de chaves. Se o alias já existir, o keytool gerará um erro, pois já existe um certificado confiável para esse alias e não importa o certificado.
2 - Se o alias apontar para uma entrada de chave, o keytool presume que você está importando uma resposta de certificado.

### Importando um novo certificado confiável
Antes de incluir o certificado no keystore, o keytool tenta verificá-lo tentando construir uma cadeia de confiança desse certificado para um certificado autoassinado (pertencente a uma CA raiz), usando certificados confiáveis que já estão disponíveis no keystore.

Se a opção -trustcacerts foi especificada, certificados adicionais são considerados para a cadeia de confiança, ou seja, os certificados em um arquivo denominado "cacerts".

Se o keytool falhar em estabelecer um caminho confiável do certificado a ser importado até um certificado autoassinado (do keystore ou do arquivo "cacerts"), as informações do certificado são impressas e o usuário é solicitado a verificá-lo, por exemplo, comparando as impressões digitais do certificado exibidas com as impressões digitais obtidas de alguma outra fonte (confiável) de informação, que pode ser o próprio proprietário do certificado. Tenha muito cuidado para garantir que o certificado seja válido antes de importá-lo como um certificado "confiável"! - consulte AVISO sobre a importação de certificados confiáveis. O usuário tem então a opção de abortar a operação de importação. Se a opção -noprompt for fornecida, no entanto, não haverá interação com o usuário.

### Importando uma resposta de certificado
Ao importar uma resposta de certificado, a resposta de certificado é validada usando certificados confiáveis do armazenamento de chaves e, opcionalmente, usando os certificados configurados no arquivo de armazenamento de chaves "cacerts" (se a opção -trustcacerts foi especificada).

Os métodos para determinar se a resposta do certificado é confiável são descritos a seguir:

- Se a resposta for um único certificado X.509, o keytool tenta estabelecer uma cadeia de confiança, começando na resposta do certificado e terminando em um certificado autoassinado (pertencente a uma CA raiz). A resposta do certificado e a hierarquia de certificados usados ​​para autenticar a resposta do certificado formam a nova cadeia de certificados de alias. Se uma cadeia de confiança não puder ser estabelecida, a resposta do certificado não será importada. Nesse caso, o keytool não imprime o certificado e solicita que o usuário o verifique, porque é muito difícil (se não impossível) para um usuário determinar a autenticidade da resposta do certificado.
- Se a resposta for uma cadeia de certificados formatados em PKCS # 7, a cadeia é ordenada primeiro (com o certificado do usuário primeiro e o certificado CA raiz autoassinado por último), antes que o keytool tente combinar o certificado CA raiz fornecido na resposta com qualquer dos certificados confiáveis ​​no armazenamento de chaves ou no arquivo de armazenamento de chaves "cacerts" (se a opção -trustcacerts foi especificada). Se nenhuma correspondência for encontrada, as informações do certificado de CA raiz são impressas, e o usuário é solicitado a verificá-lo, por exemplo, comparando as impressões digitais do certificado exibidas com as impressões digitais obtidas de alguma outra fonte (confiável) de informação, que pode ser a própria CA raiz. O usuário tem então a opção de abortar a operação de importação. Se a opção -noprompt for fornecida, no entanto, não haverá interação com o usuário.

Se a chave pública na resposta do certificado corresponder à chave pública do usuário já armazenada com o alias, a cadeia de certificados antiga será substituída pela nova cadeia de certificados na resposta. A cadeia antiga só pode ser substituída se um keypass válido, a senha usada para proteger a chave privada da entrada, for fornecido. Se nenhuma senha for fornecida e a senha da chave privada for diferente da senha do keystore, o usuário será solicitado a fazê-lo.

Este comando era denominado -import em releases anteriores. Este nome antigo ainda é suportado neste lançamento e será suportado em lançamentos futuros, mas para esclarecer o novo nome, -importcert, é preferível daqui para frente.

```
-importkeystore -srckeystore srckeystore -destkeystore destkeystore {-srcstoretype srcstoretype} 
{-deststoretype deststoretype} [-srcstorepass srcstorepass] [-deststorepass deststorepass] 
{-srcprotected} {-destprotected} {-srcalias srcalias {-destalias destalias} [-srckeypass srckeypass] 
[-destkeypass destkeypass] } {-noprompt} {-srcProviderName src_provider_name} 
{-destProviderName dest_provider_name} {-providerClass provider_class_name {-providerArg provider_arg}} 
{-v} {-protected} {-Jjavaoption}
```

Importa uma única entrada ou todas as entradas de um armazenamento de chaves de origem para um armazenamento de chaves de destino.

Quando a opção srcalias é fornecida, o comando importa a única entrada identificada pelo alias para o armazenamento de chaves de destino. Se um alias de destino não for fornecido com destalias, srcalias será usado como o alias de destino. Se a entrada de origem estiver protegida por uma senha, srckeypass será usado para recuperar a entrada. Se srckeypass não for fornecido, o keytool tentará usar srcstorepass para recuperar a entrada. Se srcstorepass não for fornecido ou estiver incorreto, o usuário será solicitado a fornecer uma senha. A entrada de destino será protegida usando destkeypass. Se destkeypass não for fornecido, a entrada de destino será protegida com a senha de entrada de origem.

Se a opção srcalias não for fornecida, todas as entradas no armazenamento de chaves de origem serão importadas para o armazenamento de chaves de destino. Cada entrada de destino será armazenada sob o alias da entrada de origem. Se a entrada de origem estiver protegida por uma senha, srcstorepass será usado para recuperar a entrada. Se srcstorepass não for fornecido ou estiver incorreto, o usuário será solicitado a fornecer uma senha. Se um tipo de entrada de armazenamento de chaves de origem não for suportado no armazenamento de chaves de destino ou se ocorrer um erro ao armazenar uma entrada no armazenamento de chaves de destino, o usuário será solicitado a ignorar a entrada e continuar ou encerrar. A entrada de destino será protegida com a senha de entrada de origem.

Se o alias de destino já existir no armazenamento de chaves de destino, o usuário será solicitado a sobrescrever a entrada ou a criar uma nova entrada com um nome de alias diferente.

Observe que se -noprompt for fornecido, o usuário não será solicitado a fornecer um novo alias de destino. As entradas existentes serão substituídas automaticamente pelo nome do alias de destino. Finalmente, as entradas que não podem ser importadas são automaticamente ignoradas e um aviso é enviado.

### Exportando dados

```
-certreq {-alias alias} {-sigalg sigalg} {-file certreq_file} [-keypass keypass] 
{-storetype storetype} {-keystore keystore} [-storepass storepass] {-providerName provider_name} 
{-providerClass provider_class_name {-providerArg provider_arg}} {-v} {-protected} {-Jjavaoption}
```
Gera uma solicitação de assinatura de certificado (CSR), usando o formato PKCS # 10.

Um CSR deve ser enviado a uma autoridade de certificação (CA). A CA autenticará o solicitante do certificado (geralmente off-line) e retornará um certificado ou cadeia de certificados, usado para substituir a cadeia de certificados existente (que consiste inicialmente em um certificado autoassinado) no armazenamento de chaves.

A chave privada e o nome distinto X.500 associados ao alias são usados ​​para criar a solicitação de certificado PKCS # 10. Para acessar a chave privada, a senha apropriada deve ser fornecida, uma vez que as chaves privadas são protegidas no armazenamento de chaves com uma senha. Se o keypass não for fornecido na linha de comando e for diferente da senha usada para proteger a integridade do keystore, o usuário será solicitado a fazê-lo.

sigalg especifica o algoritmo que deve ser usado para assinar o CSR.

O CSR é armazenado no arquivo certreq_file. Se nenhum arquivo for fornecido, o CSR é enviado para stdout.

Use o comando importcert para importar a resposta do CA.

```
-exportcert {-alias alias} {-file cert_file} {-storetype storetype} {-keystore keystore} 
[-storepass storepass] {-providerName provider_name} {-providerClass provider_class_name 
{-providerArg provider_arg}} {-rfc} {-v} {-protected} {-Jjavaoption}
```

Lê (do armazenamento de chaves) o certificado associado ao alias e o armazena no arquivo cert_file.

Se nenhum arquivo for fornecido, o certificado será enviado para stdout.

O certificado é, por padrão, emitido em codificação binária, mas, em vez disso, será emitido no formato de codificação para impressão, conforme definido pelo padrão RFC 1421 da Internet, se a opção -rfc for especificada.

Se o alias se referir a um certificado confiável, esse certificado será emitido. Caso contrário, alias se refere a uma entrada de chave com uma cadeia de certificação associada. Nesse caso, o primeiro certificado da cadeia é retornado. Este certificado autentica a chave pública da entidade endereçada pelo alias.

Este comando era denominado -export em releases anteriores. Este nome antigo ainda é suportado neste lançamento e será suportado em lançamentos futuros, mas para esclarecer o novo nome, -exportcert, é preferível daqui para frente.

### Exibindo dados
```
-list {-alias alias} {-storetype storetype} {-keystore keystore} [-storepass storepass] 
{-providerName provider_name} {-providerClass provider_class_name {-providerArg provider_arg}} 
{-v | -rfc} {-protected} {-Jjavaoption}
```

Prints (to stdout) the contents of the keystore entry identified by alias. If no alias is specified, the contents of the entire keystore are printed.

This command by default prints the MD5 fingerprint of a certificate. If the -v option is specified, the certificate is printed in human-readable format, with additional information such as the owner, issuer, serial number, and any extensions. If the -rfc option is specified, certificate contents are printed using the printable encoding format, as defined by the Internet RFC 1421 standard

You cannot specify both -v and -rfc.

```
-printcert {-file cert_file} {-v} {-Jjavaoption}
```

Lê o certificado do arquivo cert_file e imprime seu conteúdo em um formato legível. Se nenhum arquivo for fornecido, o certificado será lido do stdin.

O certificado pode ser codificado em binário ou em formato de codificação para impressão, conforme definido pelo padrão RFC 1421 da Internet.

Observação: essa opção pode ser usada independentemente de um armazenamento de chaves.

### Gerenciando o Keystore

```
-storepasswd [-new new_storepass] {-storetype storetype} {-keystore keystore} [-storepass storepass] 
{-providerName provider_name} {-providerClass provider_class_name {-providerArg provider_arg}} 
{-v} {-Jjavaoption}
Altera a senha usada para proteger a integridade do conteúdo do keystore. 
A nova senha é new_storepass, que deve ter pelo menos 6 caracteres.
```

```
-keypasswd {-alias alias} [-keypass old_keypass] [-new new_keypass] {-storetype storetype} 
{-keystore keystore} [-storepass storepass] {-providerName provider_name} 
{-providerClass provider_class_name {-providerArg provider_arg}} {-v} {-Jjavaoption}
Altera a senha sob a qual a chave privada / secreta identificada pelo alias é protegida, 
de old_keypass para new_keypass, que deve ter pelo menos 6 caracteres.
```
Se a opção -keypass não for fornecida na linha de comandos e a senha da chave for diferente da senha do keystore, o usuário será solicitado a fazê-lo.

Se a opção -new não for fornecida na linha de comando, o usuário será solicitado a fazê-lo.

```
-delete [-alias alias] {-storetype storetype} {-keystore keystore} [-storepass storepass] 
{-providerName provider_name} {-providerClass provider_class_name {-providerArg provider_arg}} 
{-v} {-protected} {-Jjavaoption}
Exclui do armazenamento de chaves a entrada identificada pelo alias. O usuário é solicitado 
a fornecer o alias, se nenhum alias for fornecido na linha de comando.
```
```
-changealias {-alias alias} [-destalias destalias] [-keypass keypass] {-storetype storetype} 
{-keystore keystore} [-storepass storepass] {-providerName provider_name} 
{-providerClass provider_class_name {-providerArg provider_arg}} {-v} {-protected} {-Jjavaoption}
Mova uma entrada de keystore existente do alias especificado para um novo alias, destalias. 
Se nenhum alias de destino for fornecido, o comando solicitará um. Se a entrada original 
estiver protegida por uma senha de entrada, a senha pode ser fornecida por meio 
da opção "-keypass". Se nenhuma senha de chave for fornecida, o storepass (se fornecido) 
será tentado primeiro. Se essa tentativa falhar, o usuário será solicitado a fornecer uma senha.
```

### Conseguindo ajuda

```
-help
```
Lista os comandos básicos e suas opções.

### Exemplos
Suponha que você queira criar um armazenamento de chaves para gerenciar seu par de chaves públicas / privadas e certificados de entidades em que você confia.

#### Gerando Seu Par de Chaves
A primeira coisa que você precisa fazer é criar um armazenamento de chaves e gerar o par de chaves. Você pode usar um comando como o seguinte:
```
keytool -genkeypair -dname "cn=Mark Jones, ou=JavaSoft, o=Sun, c=US"
      -alias business -keypass kpi135 -keystore C:\working\mykeystore
      -storepass ab987c -validity 180
```
(Observação: deve ser digitado como uma única linha. Várias linhas são usadas nos exemplos apenas para fins de legibilidade.)

Este comando cria o keystore denominado "mykeystore" no diretório "C: \ working" (presumindo que ainda não exista) e atribui a ele a senha "ab987c". Ele gera um par de chaves pública / privada para a entidade cujo "nome distinto" tem um nome comum de "Mark Jones", unidade organizacional de "JavaSoft", organização de "Sun" e código de país de duas letras "US". Ele usa o algoritmo de geração de chave "DSA" padrão para criar as chaves, ambas com 1024 bits.

Ele cria um certificado autoassinado (usando o algoritmo de assinatura "SHA1withDSA" padrão) que inclui a chave pública e as informações de nome distinto. Este certificado será válido por 180 dias e está associado à chave privada em uma entrada de armazenamento de chaves denominada pelo alias "negócio". A chave privada recebe a senha "kpi135".

O comando poderia ser significativamente mais curto se os padrões das opções fossem aceitos. Na verdade, nenhuma opção é necessária; padrões são usados ​​para opções não especificadas que possuem valores padrão e você é solicitado a fornecer quaisquer valores necessários. Assim, você poderia simplesmente ter o seguinte:

```
keytool -genkeypair
```
Nesse caso, uma entrada de keystore com alias "mykey" é criada, com um par de chaves recém-gerado e um certificado válido por 90 dias. Essa entrada é colocada no armazenamento de chaves denominado ".keystore" em seu diretório inicial. (O keystore é criado se ainda não existir.) Você será solicitado a fornecer as informações do nome distinto, a senha do keystore e a senha da chave privada.
O restante dos exemplos presume que você executou o comando -genkeypair sem opções especificadas e que respondeu aos prompts com valores iguais aos fornecidos no primeiro comando -genkeypair, acima (uma senha de chave privada "kpi135", etc.)

### Solicitando um certificado assinado de uma autoridade de certificação
Até agora, tudo o que temos é um certificado autoassinado. Um certificado tem mais probabilidade de ser confiável para outras pessoas se for assinado por uma Autoridade de Certificação (CA). Para obter essa assinatura, primeiro você gera uma Solicitação de assinatura de certificado (CSR), por meio do seguinte:

```
keytool -certreq -file MarkJ.csr
```
Isso cria um CSR (para a entidade identificada pelo alias padrão "mykey") e coloca a solicitação no arquivo denominado "MarkJ.csr". Envie este arquivo para uma CA, como a VeriSign, Inc. A CA irá autenticar você, o solicitante (geralmente off-line), e então irá retornar um certificado, assinado por eles, autenticando sua chave pública. (Em alguns casos, eles realmente retornarão uma cadeia de certificados, cada um autenticando a chave pública do signatário do certificado anterior na cadeia.)

### Importando um Certificado para a CA
Você precisa substituir seu certificado autoassinado por uma cadeia de certificados, onde cada certificado na cadeia autentica a chave pública do signatário do certificado anterior na cadeia, até uma CA "raiz".

Antes de importar a resposta do certificado de uma CA, você precisa de um ou mais "certificados confiáveis" em seu armazenamento de chaves ou no arquivo de armazenamento de chaves cacerts (que é descrito no comando importcert):

- Se a resposta do certificado for uma cadeia de certificados, você só precisa do certificado superior da cadeia (ou seja, o certificado CA "raiz" que autentica a chave pública dessa CA).
- Se a resposta do certificado for um único certificado, você precisa de um certificado para a CA emissora (aquela que o assinou), e se esse certificado não for autoassinado, você precisa de um certificado para seu signatário, e assim por diante, até um certificado de CA "raiz" autoassinado.

O arquivo de armazenamento de chaves "cacerts" é enviado com cinco certificados de CA raiz da VeriSign, portanto, você provavelmente não precisará importar um certificado da VeriSign como um certificado confiável em seu armazenamento de chaves. Mas se você solicitar um certificado assinado de uma CA diferente, e um certificado que autentica essa chave pública da CA não foi adicionado a "cacerts", você precisará importar um certificado da CA como um "certificado confiável".

Um certificado de uma CA geralmente é autoassinado ou assinado por outra CA (nesse caso, você também precisa de um certificado que autentique a chave pública dessa CA). Suponha que a empresa ABC, Inc. seja uma CA e você obtenha um arquivo chamado "ABCCA.cer" que é supostamente um certificado autoassinado da ABC, autenticando a chave pública dessa CA.

Tenha muito cuidado para garantir que o certificado seja válido antes de importá-lo como um certificado "confiável"! Visualize-o primeiro (usando o comando keytool -printcert ou o comando keytool -importcert sem a opção -noprompt) e certifique-se de que as impressões digitais do certificado exibidas correspondem às esperadas. Você pode ligar para a pessoa que enviou o certificado e comparar a (s) impressão (ões) digital (is) que você vê com as que ela mostra (ou que mostra um repositório de chave pública seguro). Somente se as impressões digitais forem iguais é que é garantido que o certificado não foi substituído em trânsito pelo certificado de outra pessoa (por exemplo, de um invasor). Se tal ataque ocorrer e você não verificar o certificado antes de importá-lo, você acabará confiando em tudo o que o invasor assinou.

Se você acredita que o certificado é válido, você pode adicioná-lo ao seu armazenamento de chaves por meio do seguinte:
```
keytool -importcert -alias abc -file ABCCA.cer
```
Isso cria uma entrada de "certificado confiável" no armazenamento de chaves, com os dados do arquivo "ABCCA.cer" e atribui o alias "abc" à entrada.

### Importando a resposta do certificado da CA
Depois de importar um certificado que autentica a chave pública da CA para a qual enviou sua solicitação de assinatura de certificado (ou já existe tal certificado no arquivo "cacerts"), você pode importar a resposta do certificado e, assim, substituir seu certificado autoassinado com uma cadeia de certificados. Esta cadeia é aquela retornada pela CA em resposta à sua solicitação (se a resposta da CA for uma cadeia), ou uma construída (se a resposta da CA for um único certificado) usando a resposta do certificado e certificados confiáveis que já estão disponíveis no keystore onde você importa a resposta ou no arquivo keystore "cacerts".

Por exemplo, suponha que você tenha enviado sua solicitação de assinatura de certificado à VeriSign. Você pode então importar a resposta por meio do seguinte, que presume que o certificado retornado é denominado "VSMarkJ.cer":
```
keytool -importcert -trustcacerts -file VSMarkJ.cer
```
### Exportando um certificado que autentica sua chave pública
Suponha que você tenha usado a ferramenta jarsigner para assinar um arquivo Java ARchive (JAR). Os clientes que desejam usar o arquivo irão querer autenticar sua assinatura.
Uma maneira de fazer isso é primeiro importando seu certificado de chave pública para o armazenamento de chaves como uma entrada "confiável". Você pode exportar o certificado e fornecê-lo aos seus clientes. Como exemplo, você pode copiar seu certificado para um arquivo denominado MJ.cer por meio do seguinte, supondo que a entrada tenha o alias de "mykey":
```
keytool -exportcert -alias mykey -file MJ.cer
```
Dado esse certificado e o arquivo JAR assinado, um cliente pode usar a ferramenta jarsigner para autenticar sua assinatura.

Importando Keystore
O comando "importkeystore" é usado para importar um keystore inteiro para outro keystore, o que significa que todas as entradas do keystore de origem, incluindo chaves e certificados, são todas importadas para o keystore de destino em um único comando. Você pode usar este comando para importar entradas de um tipo diferente de armazenamento de chaves. Durante a importação, todas as novas entradas no armazenamento de chaves de destino terão os mesmos nomes de alias e senhas de proteção (para chaves secretas e chaves privadas). Se o keytool tiver dificuldades para recuperar as chaves privadas ou chaves secretas do armazenamento de chaves de origem, ele solicitará uma senha. Se detectar a duplicação de alias, ele solicitará um novo, você pode especificar um novo alias ou simplesmente permitir que o keytool sobrescreva o existente.

Por exemplo, para importar entradas de um keystore key.jks do tipo JKS normal para um keystore baseado em hardware do tipo PKCS#11, você pode usar o comando:
```
keytool -importkeystore
    -srckeystore key.jks -destkeystore NONE
    -srcstoretype JKS -deststoretype PKCS11
    -srcstorepass changeit -deststorepass topsecret
```
O comando importkeystore também pode ser usado para importar uma única entrada de um armazenamento de chaves de origem para um armazenamento de chaves de destino. Neste caso, além das opções que você vê no exemplo acima, você precisa especificar o alias que deseja importar. Com a opção srcalias fornecida, você também pode especificar o nome do apelido do destino na linha de comando, bem como a senha de proteção para uma chave secreta / privada e a senha de proteção de destino desejada. Dessa forma, você pode emitir um comando keytool que nunca fará uma pergunta. Isso torna muito conveniente incluir um comando keytool em um arquivo de script, como este:
```
keytool -importkeystore
    -srckeystore key.jks -destkeystore NONE
    -srcstoretype JKS -deststoretype PKCS11
    -srcstorepass changeit -deststorepass topsecret
    -srcalias myprivatekey -destalias myoldprivatekey
    -srckeypass oldkeypass -destkeypass mynewkeypass
    -noprompt
```
### Terminologia e avisos
#### KeyStore
Um keystore é um recurso de armazenamento para chaves criptográficas e certificados.

### KeyStore Entries
Os keystores podem ter diferentes tipos de entradas. Os dois tipos de entrada mais aplicáveis para keytool incluem:
- Entradas de chave - cada uma contém informações de chave criptográfica muito confidenciais, que são armazenadas em um formato protegido para evitar acesso não autorizado. Normalmente, uma chave armazenada neste tipo de entrada é uma chave secreta ou uma chave privada acompanhada pela "cadeia" de certificados para a chave pública correspondente. O keytool pode lidar com os dois tipos de entrada, enquanto a ferramenta jarsigner apenas lida com o último tipo de entrada, ou seja, as chaves privadas e suas cadeias de certificados associadas.
- Entradas de certificados confiáveis - cada uma contém um único certificado de chave pública pertencente a outra parte. É chamado de "certificado confiável" porque o proprietário do keystore confia que a chave pública no certificado realmente pertence à identidade identificada pelo "assunto" (proprietário) do certificado. O emissor do certificado atesta isso, assinando o certificado.

### KeyStore Aliases
Todas as entradas de keystore (entradas de chave e certificados confiáveis) são acessadas por meio de aliases exclusivos.

Um alias é especificado quando você adiciona uma entidade ao armazenamento de chaves usando o comando -genseckey para gerar uma chave secreta, o comando -genkeypair para gerar um par de chaves (chave pública e privada) ou o comando -importcert para adicionar um certificado ou cadeia de certificados a a lista de certificados confiáveis. Os comandos keytool subsequentes devem usar esse mesmo alias para se referir à entidade.

Por exemplo, suponha que você use o alias duke para gerar um novo par de chaves pública / privada e envolver a chave pública em um certificado autoassinado (consulte Cadeias de certificados) por meio do seguinte comando:
```
keytool -genkeypair -alias duke -keypass dukekeypasswd
```
Isso especifica uma senha inicial de "dukekeypasswd" exigida pelos comandos subsequentes para acessar a chave privada associada ao apelido duke. Se mais tarde você quiser alterar a senha da chave privada do duque, use um comando como o seguinte:
```
keytool -keypasswd -alias duke -keypass dukekeypasswd -new newpass
```
Isso muda a senha de "dukekeypasswd" para "newpass".
Observação: Na verdade, uma senha não deve ser especificada em uma linha de comando ou em um script, a menos que seja para fins de teste ou se você estiver em um sistema seguro. Se você não especificar uma opção de senha obrigatória em uma linha de comando, ela será solicitada.

### Implementação de KeyStore
A classe KeyStore fornecida no pacote java.security fornece interfaces bem definidas para acessar e modificar as informações em um armazenamento de chaves. É possível que haja várias implementações concretas diferentes, em que cada implementação é aquela para um tipo específico de armazenamento de chaves.
Atualmente, duas ferramentas de linha de comando (keytool e jarsigner) e uma ferramenta baseada em GUI chamada Policy Tool fazem uso de implementações de keystore. Como o KeyStore está disponível publicamente, os usuários podem escrever aplicativos de segurança adicionais que o utilizem.

Há uma implementação padrão integrada, fornecida pela Sun Microsystems. Ele implementa o armazenamento de chave como um arquivo, utilizando um tipo de armazenamento de chave proprietário (formato) denominado "JKS". Ele protege cada chave privada com sua senha individual e também protege a integridade de todo o armazenamento de chaves com uma senha (possivelmente diferente).

As implementações de keystore são baseadas no provedor. Mais especificamente, as interfaces de aplicativo fornecidas pelo KeyStore são implementadas em termos de uma "Service Provider Interface" (SPI). Ou seja, há uma classe KeystoreSpi abstrata correspondente, também no pacote java.security, que define os métodos da Interface do provedor de serviços que os "provedores" devem implementar. (O termo "provedor" se refere a um pacote ou conjunto de pacotes que fornecem uma implementação concreta de um subconjunto de serviços que podem ser acessados ​​pela API de segurança Java.) Assim, para fornecer uma implementação de keystore, os clientes devem implementar um "provedor "e fornecer uma implementação de subclasse KeystoreSpi, conforme descrito em Como implementar um provedor para a arquitetura de criptografia Java.

Os aplicativos podem escolher diferentes tipos de implementações de keystore de diferentes provedores, usando o método de fábrica "getInstance" fornecido na classe KeyStore. Um tipo de armazenamento de chave define o armazenamento e o formato de dados das informações do armazenamento de chave e os algoritmos usados ​​para proteger as chaves privadas / secretas no armazenamento de chave e a integridade do próprio armazenamento de chave. Implementações de keystore de diferentes tipos não são compatíveis.

keytool funciona em qualquer implementação de keystore baseada em arquivo. (Ele trata o local do keytore que é passado para ele na linha de comando como um nome de arquivo e o converte em um FileInputStream, a partir do qual carrega as informações do keystore.) As ferramentas jarsigner e policytool, por outro lado, podem ler um keystore de qualquer local que pode ser especificado usando um URL.

Para keytool e jarsigner, você pode especificar um tipo de keystore na linha de comando, por meio da opção -storetype. Para a ferramenta de política, você pode especificar um tipo de armazenamento de chave por meio do menu "Armazenamento de chave".

Se você não especificar explicitamente um tipo de armazenamento de chave, as ferramentas escolherão uma implementação de armazenamento de chave com base simplesmente no valor da propriedade keystore.type especificada no arquivo de propriedades de segurança. O arquivo de propriedades de segurança é chamado de java.security e reside no diretório de propriedades de segurança, java.home \ lib \ security, em que java.home é o diretório do ambiente de tempo de execução (o diretório jre no SDK ou o diretório de nível superior do o Java 2 Runtime Environment).

Cada ferramenta obtém o valor keystore.type e examina todos os provedores atualmente instalados até encontrar um que implemente armazenamentos de chaves desse tipo. Em seguida, ele usa a implementação de armazenamento de chaves desse provedor.

A classe KeyStore define um método estático denominado getDefaultType que permite que aplicativos e miniaplicativos recuperem o valor da propriedade keystore.type. A linha de código a seguir cria uma instância do tipo de keystore padrão (conforme especificado na propriedade keystore.type):
```
KeyStore keyStore = KeyStore.getInstance(KeyStore.getDefaultType());
```
O tipo de keystore padrão é "jks" (o tipo proprietário da implementação de keystore fornecida pela Sun). Isso é especificado pela seguinte linha no arquivo de propriedades de segurança:
```
keystore.type=jks
```
Para que as ferramentas utilizem uma implementação de keystore diferente do padrão, você pode alterar essa linha para especificar um tipo de keystore diferente.

Por exemplo, se você tiver um pacote de provedor que fornece uma implementação de armazenamento de chave para um tipo de armazenamento de chave chamado "pkcs12", altere a linha para
```
keystore.type=pkcs12
```


