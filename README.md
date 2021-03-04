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



