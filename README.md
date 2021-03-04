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
Usado para identificar o nome de um provedor de serviços criptográficos quando listado no arquivo de propriedades de segurança.
```

```
-providerClass provider_class_name
Usado para especificar o nome do arquivo de classe principal do provedor de serviços criptográficos quando o provedor de serviços não está listado no arquivo de propriedades de segurança.
```

```
-providerArg provider_arg
Usado em conjunto com -providerClass. Representa um argumento de entrada de string opcional para o construtor de provider_class_name.
```
```
-protected
Ou true ou false. Este valor deve ser especificado como verdadeiro se uma senha deve ser fornecida por meio de um caminho de autenticação protegido, como um leitor de PIN dedicado.
```

### Comandos
#### Criação ou adição de dados ao armazenamento de chaves

```
-genkeypair {-alias alias} {-keyalg keyalg} {-keysize keysize} {-sigalg sigalg} [-dname dname] [-keypass keypass] {-validity valDays}
{-storetype storetype} {-keystore keystore} [-storepass storepass] {-providerClass provider_class_name {-providerArg provider_arg}} {-v} 
{-protected} {-Jjavaoption}
```

Gera um par de chaves (uma chave pública e uma chave privada associada). Envolve a chave pública em um certificado autoassinado X.509 v3, que é armazenado como uma cadeia de certificados de elemento único. Esta cadeia de certificados e a chave privada são armazenadas em uma nova entrada de armazenamento de chaves identificada por alias.

keyalg especifica o algoritmo a ser usado para gerar o par de chaves e keysize especifica o tamanho de cada chave a ser gerada. sigalg especifica o algoritmo que deve ser usado para assinar o certificado autoassinado; este algoritmo deve ser compatível com keyalg.

dname especifica o Nome distinto X.500 a ser associado ao alias e é usado como o emissor e os campos de assunto no certificado autoassinado. Se nenhum nome distinto for fornecido na linha de comando, o usuário será solicitado a fornecer um.

keypass é uma senha usada para proteger a chave privada do par de chaves gerado. Se nenhuma senha for fornecida, o usuário será solicitado a inseri-la. Se você pressionar RETURN no prompt, a senha da chave será definida com a mesma senha usada para o armazenamento de chaves. o keypass deve ter pelo menos 6 caracteres.

valDays informa o número de dias durante os quais o certificado deve ser considerado válido.

Este comando era denominado -genkey em versões anteriores. Este nome antigo ainda é suportado neste lançamento e será suportado em lançamentos futuros, mas para esclarecer o novo nome, -genkeypair, é preferível daqui para frente.

```
-genseckey {-alias alias} {-keyalg keyalg} {-keysize keysize} [-keypass keypass] {-storetype storetype} {-keystore keystore} 
[-storepass storepass] {-providerClass provider_class_name {-providerArg provider_arg}} {-v} {-protected} {-Jjavaoption}
Generates a secret key and stores it in a new KeyStore.SecretKeyEntry identified by alias.
```
"keyalg especifica o algoritmo a ser usado para gerar a chave secreta e keysize especifica o tamanho da chave a ser gerada. keypass é uma senha usada para proteger a chave secreta. Se nenhuma senha for fornecida, o usuário será solicitado a inseri-la. Se você pressionar RETURN no prompt, a senha da chave será definida com a mesma senha usada para o armazenamento de chaves. o keypass deve ter pelo menos 6 caracteres."

```
-importcert {-alias alias} {-file cert_file} [-keypass keypass] {-noprompt} {-trustcacerts} {-storetype storetype} {-keystore keystore}
[-storepass storepass] {-providerName provider_name} {-providerClass provider_class_name {-providerArg provider_arg}} {-v} {-protected} 
{-Jjavaoption}
```



