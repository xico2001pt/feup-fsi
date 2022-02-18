# Trabalho realizado na Semana #3

## Identificação

- **CVE-ID**: CVE-2020-14882
- **Descrição geral**: Vulnerabilidade que permite execução arbitrária de código em algumas versões do Oracle WebLogic Server através de pedidos HTTP.
- Estes pedidos podem ser realizados por qualquer atacante não autenticado com acesso à rede e podem permitir controlo total do Oracle WebLogic Server por parte de um atacante.
- As versões afetadas são as seguintes: 10.3.6.0.0, 12.1.3.0.0, 12.2.1.3.0, 12.2.1.4.0 e 14.1.1.0.0

## Catalogação
- Esta vulnerabilidade foi [reportada](https://www.oracle.com/security-alerts/cpuoct2020traditional.html) à empresa Oracle pelo investigador Voidfyoo pertecente ao Chaitin Security Lab e corrigida no *Critical Patch Update* (*CPU*) de Outubro de 2020.
- O CVE foi originalmente publicado a 21 de Outubro de 2020. Dentro de uma semana, a 28 de Outubro foi publicado um [*Proof of Concept* exploit](https://testbnull.medium.com/weblogic-rce-by-only-one-get-request-cve-2020-14882-analysis-6e4b09981dbf) (PoC) e, apenas passado um dia, foram registados ataques baseados nele.
- O CVE-2020-14882 apresenta um score CVSS 3.1 de 9.8, o que é considerado **crítico**.
- A Oracle não apresenta nenhuma programa de bug bounty, as vulnerabilidades de segurança devem ser [submetidas por email](https://www.oracle.com/corporate/security-practices/assurance/vulnerability/reporting.html) sendo a única recompensação a atribuição de crédito no *CPU* onde a vulnerabilidade for patched.

## Exploit
-  Esta vulnerabilidade pode ser exploited através de [scripts](https://packetstormsecurity.com/files/159769/Oracle-WebLogic-Server-Remote-Code-Execution.html) ou manualmente através de URLs contendo um payload Java executável `http://target_url/console/images/%252E%252E%252Fconsole.portal?_nfpb=false&_pageLable=&handle=com.tangosol.coherence.mvel2.sh.ShellSession(PAYLOAD);`  onde o payload pode ser, por exemplo, `\"java.lang.Runtime.getRuntime().exec('"cat /etc/passwd"');\"`.
- Os pontos críticos deste exploit são o caminho relativo `/console/images/%252E%252E%252Fconsole.portal`, que permite a execução do segundo componente sem qualquer tipo de autenticação ou restrição e um payload injetado no parametro handle, como demonstrado anteriormente.
- Este exploit pode ser automatizado através do uso do módulo [exploit/multi/http/weblogic_admin_handle_rce](https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/multi/http/weblogic_admin_handle_rce.rb) no [Metasploit](https://www.metasploit.com/).

## Ataques
- Apenas um dia após o lançamento do primeiro PoC, o investigador Johannes B. Ullrich, pertencente ao SANS Technology Institute, detetou [tentativas de ataque](https://isc.sans.edu/forums/diary/PATCH+NOW+CVE202014882+Weblogic+Actively+Exploited+Against+Honeypots/26734/) contra os seus honeypots.
- Dentro desses ataques foram encontrados [exploits](https://isc.sans.edu/forums/diary/Attackers+Exploiting+WebLogic+Servers+via+CVE202014882+to+install+Cobalt+Strike/26752) cujo objetivo é instalar payloads de [Cobalt Strike](https://www.cobaltstrike.com/), uma ferramenta especializada em testes de intrusão.
- A vulnerabilidade foi explorada pelo [DarkIRC](https://securityaffairs.co/wordpress/111743/hacking/darkirc-oracle-weblogic-cve-2020-14882.htm), um botnet que pode lançar ataques de denial of service, executar comandos e fazer download de ficheiros em sistemas infetados. O malware por trás do botnet foi publicitado para venda por 75$ num fórum de hacking, [Hack Forums](https://hackforums.net/index.php).
