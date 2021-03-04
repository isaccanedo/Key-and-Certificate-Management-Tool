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



