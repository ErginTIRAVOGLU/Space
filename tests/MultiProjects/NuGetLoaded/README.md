# NuGetLoaded Manual Test Projects

Bu klas�r Space NuGet paketlerinin publish edildikten sonra manuel olarak test edilmesi i�in haz?rlanm??t?r. Ama� repository i�indeki kaynak projelere de?il yay?mlanan paketlere referans vermektir.

## Projeler
- ApiHost (net8.0 Web API) - Root aggregator ( `<SpaceGenerateRootAggregator>true</SpaceGenerateRootAggregator>` )
- LibAlpha (class library)
- LibBeta (class library)

LibAlpha ve LibBeta kendi handlerlar?n? i�erir. ApiHost bu iki projeyi referans ederek source generator'un root projede t�m handlerlar? toplamas?n? sa?lar.

## Kullan?m
Varsay?lan olarak repo i�indeki projelere referans verilir (ProjectReference). Yay?nlanan NuGet paketlerini test etmek i�in derleme s?ras?nda a?a??daki parametreyi verin:

```
dotnet build -c Release /p:UseLocalSpaceProjects=false
```

Ya da `ApiHost`, `LibAlpha`, `LibBeta` projelerinin `.csproj` dosyalar?ndaki `UseLocalSpaceProjects` �zelli?ini false yap?n.

NuGet paket versiyonlar?n? test etmek istedi?iniz ger�ek versiyonlarla de?i?tirin (�rn: `1.0.0-preview.3`).

## �rnek �al??t?rma
```
dotnet run --project tests/MultiProjects/NuGetLoaded/ApiHost/ApiHost.csproj
```

### �rnek ?stekler
```
GET http://localhost:5000/alpha/hello        -> AlphaHandled:hello
GET http://localhost:5000/beta/5             -> Beta:5
GET http://localhost:5000/multi/hi           -> AlphaHandled:hi (veya override senaryosu i�in ileride isimlendirilmi? handler �a?r?s? eklenebilir)
GET http://localhost:5000/notify/3           -> 3 adet notification publish eder
```

## Notlar
- Paket testinde `Space.Modules.InMemoryCache` gibi ekstra mod�lleri de eklemek isterseniz ilgili PackageReference sat?r?n? a�?n.
- Root projede `<SpaceGenerateRootAggregator>true</SpaceGenerateRootAggregator>` olmas? di?er class library projelerindeki handlerlar?n toplanmas? i�in gereklidir.
- Handler / Pipeline say?s?n? artt?rarak ger�ek senaryolar? manuel test edebilirsiniz.
