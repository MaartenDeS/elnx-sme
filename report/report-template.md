# Enterprise Linux Lab Report

- Student name: Maarten De Smedt
- Github repo: <https://github.com/HoGentTIN/elnx-sme-MaartenDeS>


Het opzetten van de basisintellingen in ansible voor alle servers.

## Test plan

De server kan getest worden doormiddel van de testscripts.

## Procedure/Documentation

Eerst worden de vereiste rollen toegevoegd in het site.yml bestand bijgevoegd.

Eerst voegde ik in het all.yml bestand de EPEL Repository en de packages toe. In dit bestand voegde ik ook de user Maarten toe met wachtwoord maarten. Deze user is een admin. De motd werd ook aangezet. En ten laatste voegde ik de rsa key toe. Dit deed ik met commando %%ssh-keygen%%.

Deze server had ook aangepaste certificaten nodig. Deze diende toen gekopieerd te worden in de correcte mappen op de server. 



## Test report

The test report is a transcript of the execution of the test plan, with the actual results. Significant problems you encountered should also be mentioned here, as well as any solutions you found. The test report should clearly prove that you have met the requirements.

## Resources

List all sources of useful information that you encountered while completing this assignment: books, manuals, HOWTO's, blog posts, etc.
