---
title: "NoSQL 자습서: DocumentDB .NET Core SDK | Microsoft Docs"
description: "DocumentDB .NET Core SDK를 사용하여 온라인 데이터베이스 및 C# 콘솔 응용 프로그램을 만드는 NoSQL 자습서입니다. DocumentDB는 JSON에 대한 NoSQL 데이터베이스입니다."
services: documentdb
documentationcenter: .net
author: arramac
manager: jhubbard
editor: 
ms.assetid: 9f93e276-9936-4efb-a534-a9889fa7c7d2
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 02/11/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: fba82c5c826da7d1912814b61c5065ca7f726011
ms.openlocfilehash: e31be1a97b9b3d7718e4e6f58f22a2c6f5550beb
ms.lasthandoff: 02/23/2017


---
# <a name="nosql-tutorial-build-a-documentdb-c-console-application-on-net-core"></a>NoSQL 자습서: .NET Core에서 DocumentDB C# 콘솔 응용 프로그램 빌드
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [Node.JS](documentdb-nodejs-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Azure DocumentDB .NET Core SDK용 NoSQL 자습서를 시작합니다. 이 자습서를 따라 하면 DocumentDB 리소스를 만들고 쿼리하는 콘솔 응용 프로그램이 생깁니다.

다음에 대해 설명합니다.

* DocumentDB 계정 만들기 및 연결
* Visual Studio 솔루션 구성
* 온라인 데이터베이스 만들기
* 컬렉션 만들기
* JSON 문서 만들기
* 컬렉션 쿼리
* 문서 바꾸기
* 문서 삭제
* 데이터베이스 삭제

시간이 없으십니까? 염려하지 마십시오. [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started)에서 전체 솔루션을 사용할 수 있습니다. 빠른 지침을 보려면 [전체 솔루션 다운로드 섹션](#GetSolution) 으로 이동하세요.

나중에 이 페이지 위쪽 또는 아래쪽에 있는 응답 단추를 통해 의견을 보내주세요. 직접 연락을 받고 싶은 경우 설명에 메일 주소를 포함하세요.

> [!NOTE]
> 이 자습서에서 사용되는 DocumentDB .NET Core SDK는 UWP(유니버설 Windows 플랫폼) 앱과 호환되지 않습니다. UWP 앱을 지원하는 .NET Core SDK의 미리 보기 버전은 [askdocdb@microsoft.com](mailto:askdocdb@microsoft.com)(으)로 전자 메일을 보내세요.

이제 시작하겠습니다.

## <a name="prerequisites"></a>필수 조건
다음 항목이 있는지 확인합니다.

* 활성 Azure 계정. 계정이 없는 경우 [무료 계정](https://azure.microsoft.com/free/)에 등록할 수 있습니다. 
    * 또는 이 자습서에 [Azure DocumentDB 에뮬레이터](documentdb-nosql-local-emulator.md)를 사용할 수 있습니다.
* [Visual Studio 2015 업데이트 3](https://go.microsoft.com/fwlink/?LinkId=691129) 및 [.NET Core 1.0.1 - VS 2015 도구 미리 보기 2](https://go.microsoft.com/fwlink/?LinkID=827546)
    * MacOS 또는 Linux에서 작업하는 경우 원하는 플랫폼에 대한 [.NET Core SDK](https://www.microsoft.com/net/core#macos)를 설치하여 명령줄에서 .NET Core 앱을 개발할 수 있습니다. 
    * Windows에서 작업하는 경우 [.NET Core SDK](https://www.microsoft.com/net/core#windows)를 설치하여 명령줄에서 .NET Core 앱을 개발할 수 있습니다. 
    * 자체 편집기를 사용하거나 Windows, Linux, MacOS에서 작동하는 무료 [Visual Studio Code](https://code.visualstudio.com/)를 다운로드할 수 있습니다. 

## <a name="step-1-create-a-documentdb-account"></a>1단계: DocumentDB 계정 만들기
DocumentDB 계정을 만들어 보겠습니다. 계정이 이미 있는 경우 [Visual Studio 솔루션 설치](#SetupVS)로 건너뛸 수 있습니다. DocumentDB 에뮬레이터를 사용할 경우 [Azure DocumentDB 에뮬레이터](documentdb-nosql-local-emulator.md)의 단계에 따라 에뮬레이터를 설정하고 [Visual Studio 솔루션 설치](#SetupVS)로 건너뜁니다.

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a name="a-idsetupvsastep-2-setup-your-visual-studio-solution"></a><a id="SetupVS"></a>2단계: Visual Studio 솔루션 설치
1. 컴퓨터에서 **Visual Studio 2015** 를 엽니다.
2. **파일** 메뉴에서 **새로 만들기**와 **프로젝트**를 차례로 선택합니다.
3. **새 프로젝트** 대화 상자에서 **템플릿** / **Visual C#** / **.NET Core**/**콘솔 응용 프로그램(.NET Core)**을 선택하고 프로젝트 이름을 지정한 후 **확인**을 클릭합니다.
   ![새 프로젝트 창의 스크린샷](./media/documentdb-dotnetcore-get-started/nosql-tutorial-new-project-2.png)
4. **솔루션 탐색기**에서 Visual Studio 솔루션 아래에 있는 새 콘솔 응용 프로그램을 마우스 오른쪽 단추로 클릭합니다.
5. 메뉴를 종료하지 않고 **NuGet 패키지 관리...**
   ![프로젝트의 마우스 오른쪽 단추 클릭 메뉴의 스크린샷](./media/documentdb-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)을 클릭합니다.
6. **Nuget** 탭에서 **찾아보기**를 클릭하고 검색 상자에 **azure documentdb**를 입력합니다.
7. 결과 내에서 **Microsoft.Azure.DocumentDB.Core**를 찾아 **설치**를 클릭합니다.
   .NET Core용 DocumentDB 클라이언트 라이브러리의 패키지 ID는 [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)입니다. 이 .NET Core Nuget 패키지에서 지원되지 않는 .NET Framework 버전(예: net461)을 대상으로 하는 경우 .NET Framework 4.5 이후의 모든 .NET Framework 버전을 지원하는 [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB)를 사용합니다.

잘하셨습니다. 설치를 완료했으므로 코드를 작성해 보겠습니다. [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started)에서 이 자습서의 완성된 코드 프로젝트를 찾을 수 있습니다.

## <a name="a-idconnectastep-3-connect-to-a-documentdb-account"></a><a id="Connect"></a>3단계: DocumentDB 계정에 연결
먼저 Program.cs에서 C# 응용 프로그램의 시작 부분에 다음 참조를 추가합니다.

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> 이 NoSQL 자습서를 완료하려면 위의 종속성을 추가했는지 확인합니다.
> 
> 

이제, 두 가지 상수와 *클라이언트* 변수를 공용 클래스 *프로그램* 아래에 추가합니다.

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUri = "<your endpoint URI>";
        private const string PrimaryKey = "<your key>";
        private DocumentClient client;

다음으로 [Azure 포털](https://portal.azure.com) 로 이동하여 URI 및 기본 키를 검색합니다. DocumentDB URI 및 기본 키는 응용 프로그램에서 연결할 곳을 이해하고 DocumentDB에서 응용 프로그램 연결을 신뢰하는 데 필요합니다.

Azure 포털에서 DocumentDB 계정으로 이동한 다음 **키**를 클릭합니다.

포털에서 URI를 복사하고 program.cs 파일의 `<your endpoint URI>` 에 붙여 넣습니다. 그런 다음 포털에서 기본 키를 복사하고 `<your key>`에 붙여 넣습니다. Azure DocumentDB 에뮬레이터를 사용하는 경우 끝점으로 `https://localhost:8081`과, [DocumentDB 에뮬레이터를 사용한 개발 방법](documentdb-nosql-local-emulator.md)에서 잘 정의된 권한 부여 키를 사용합니다.

![C# 콘솔 응용 프로그램을 만들기 위해 NoSQL 자습서에서 사용하는 Azure 포털의 스크린샷 DocumentDB 계정 블레이드의 키 단추 및 키 블레이드의 URI, 기본 키 및 보조키 값이 강조 표시된 DocumentDB 계정을 보여 줌][keys]

**DocumentClient**의 새 인스턴스를 만드는 것으로 시작 응용 프로그램을 시작해 보겠습니다.

**Main** 메서드 아래에 **GetStartedDemo**라는 이름의 새 비동기 작업을 추가하면, 새 **DocumentClient**가 인스턴스화됩니다.

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
    }

다음 코드를 추가하여 **Main** 메서드에서 비동기 작업을 실행합니다. **Main** 메서드가 예외를 catch하여 콘솔에 기록합니다.

    static void Main(string[] args)
    {
            // ADD THIS PART TO YOUR CODE
            try
            {
                    Program p = new Program();
                    p.GetStartedDemo().Wait();
            }
            catch (DocumentClientException de)
            {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
            }
            catch (Exception e)
            {
                    Exception baseException = e.GetBaseException();
                    Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
            }
            finally
            {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
            }

**F5** 키를 눌러 응용 프로그램을 실행합니다.

축하합니다. DocumentDB 계정에 성공적으로 연결되었으므로 DocumentDB 리소스 작업에 대해 살펴보겠습니다.  

## <a name="step-4-create-a-database"></a>4단계: 데이터베이스 만들기
데이터베이스 생성을 위한 코드를 추가하기 전에, 콘솔에 쓰기 위한 도우미 메서드를 추가합니다.

**GetStartedDemo** 메서드에 **WriteToConsoleAndPromptToContinue** 메서드를 복사하여 붙여넣습니다.

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

**DocumentClient** 클래스의 [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) 메서드를 사용하여 DocumentDB [데이터베이스](documentdb-resources.md#databases)를 만들 수 있습니다. 데이터베이스는 여러 컬렉션으로 분할된 JSON 문서 저장소의 논리적 컨테이너입니다.

클라이언트 생성의 **GetStartedDemo** 메서드에 다음 코드를 복사하여 붙여넣습니다. *FamilyDB*라는 데이터베이스가 생성됩니다.

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });

**F5** 키를 눌러 응용 프로그램을 실행합니다.

축하합니다. DocumentDB 데이터베이스가 성공적으로 생성되었습니다.  

## <a name="a-idcreatecollastep-5-create-a-collection"></a><a id="CreateColl"></a>5단계: 컬렉션 만들기
> [!WARNING]
> **CreateDocumentCollectionAsync** 는 가격 책정 의미가 포함된 예약된 처리량이 있는 새 컬렉션을 만듭니다. 자세한 내용은 [가격 페이지](https://azure.microsoft.com/pricing/details/documentdb/)를 참조하세요.
> 
> 

**DocumentClient** 클래스의 [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) 메서드를 사용하여 [컬렉션](documentdb-resources.md#collections)을 만들 수 있습니다. 컬렉션은 JSON 문서 및 관련 JavaScript 응용 프로그램 논리의 컨테이너입니다.

데이터베이스 생성의 **GetStartedDemo** 메서드에 다음 코드를 복사하여 붙여넣습니다. *FamilyCollection_oa*라는 문서 컬렉션이 생성됩니다.

        this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

        await this.CreateDatabaseIfNotExists("FamilyDB_oa");

        // ADD THIS PART TO YOUR CODE
        await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

**F5** 키를 눌러 응용 프로그램을 실행합니다.

축하합니다. DocumentDB 문서 컬렉션이 성공적으로 생성됩니다.  

## <a name="a-idcreatedocastep-6-create-json-documents"></a><a id="CreateDoc"></a>6단계: JSON 문서 만들기
**DocumentClient** 클래스의 [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) 메서드를 사용하여 [문서](documentdb-resources.md#documents)를 만들 수 있습니다. 문서는 사용자 정의(임의) JSON 콘텐츠입니다. 이제 하나 이상의 문서를 삽입할 수 있습니다. 데이터베이스에 저장하려는 데이터가 이미 있다면 DocumentDB의 [데이터 마이그레이션 도구](documentdb-import-data.md)를 사용할 수 있습니다.

먼저 이 샘플에서는 DocumentDB 내에 저장된 개체를 나타내는 **가족** 클래스를 만들어야 합니다. 또한 **가족** 내에서 사용되는 **부모**, **자식**, **애완 동물**, **주소** 하위 클래스를 만듭니다. 문서에는 JSON에서 **ID**로 직렬화된 **ID** 속성이 있어야 합니다. **GetStartedDemo** 메서드 다음에 다음 내부 하위 클래스를 추가하여 이러한 클래스를 만듭니다.

**WriteToConsoleAndPromptToContinue** 메서드에 **가족**, **부모**, **자식**, **애완 동물** 및 **주소** 클래스를 복사하여 붙여 넣습니다.

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }

    // ADD THIS PART TO YOUR CODE
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
                return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }

**CreateDocumentCollectionIfNotExists** 메서드에 **CreateFamilyDocumentIfNotExists** 메서드를 복사하여 붙여넣습니다.

    // ADD THIS PART TO YOUR CODE
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
            }
            else
            {
                throw;
            }
        }
    }

그리고 Andersen Family와 Wakefield Family의 문서 하나씩 두 개의 문서를 삽입합니다.

문서 컬렉션 생성의 **GetStartedDemo** 메서드에 다음 코드를 복사하여 붙여넣습니다.

    await this.CreateDatabaseIfNotExists("FamilyDB_oa");

    await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

    // ADD THIS PART TO YOUR CODE
    Family andersenFamily = new Family
    {
            Id = "Andersen.1",
            LastName = "Andersen",
            Parents = new Parent[]
            {
                    new Parent { FirstName = "Thomas" },
                    new Parent { FirstName = "Mary Kay" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FirstName = "Henriette Thaulow",
                            Gender = "female",
                            Grade = 5,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Fluffy" }
                            }
                    }
            },
            Address = new Address { State = "WA", County = "King", City = "Seattle" },
            IsRegistered = true
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

    Family wakefieldFamily = new Family
    {
            Id = "Wakefield.7",
            LastName = "Wakefield",
            Parents = new Parent[]
            {
                    new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                    new Parent { FamilyName = "Miller", FirstName = "Ben" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FamilyName = "Merriam",
                            FirstName = "Jesse",
                            Gender = "female",
                            Grade = 8,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Goofy" },
                                    new Pet { GivenName = "Shadow" }
                            }
                    },
                    new Child
                    {
                            FamilyName = "Miller",
                            FirstName = "Lisa",
                            Gender = "female",
                            Grade = 1
                    }
            },
            Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
            IsRegistered = false
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

**F5** 키를 눌러 응용 프로그램을 실행합니다.

축하합니다. 두 개의 DocumentDB 문서가 성공적으로 생성되었습니다.  

![NoSQL에서 C# 콘솔 응용 프로그램을 만들기 위해 사용한 계정, 데이터베이스, 컬렉션 및 문서 간의 계층 관계를 보여 주는 다이어그램](./media/documentdb-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a name="a-idqueryastep-7-query-documentdb-resources"></a><a id="Query"></a>7단계: DocumentDB 리소스 쿼리
DocumentDB는 각 컬렉션에 저장된 JSON 문서에 대해 [다양한 쿼리](documentdb-sql-query.md)를 지원합니다.  다음 샘플 코드는 DocumentDB SQL 구문뿐 아니라 LINQ를 사용하는 다양한 쿼리를 보여 줍니다. 이러한 쿼리는 이전 단계에서 삽입한 문서에 대해 실행할 수 있습니다.

**CreateFamilyDocumentIfNotExists** 메서드에 **ExecuteSimpleQuery** 메서드를 복사하여 붙여넣습니다.

    // ADD THIS PART TO YOUR CODE
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

            // Here we find the Andersen family via its LastName
            IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(f => f.LastName == "Andersen");

            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (Family family in familyQuery)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            // Now execute the same query via direct SQL
            IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                    queryOptions);

            Console.WriteLine("Running direct SQL query...");
            foreach (Family family in familyQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

두 번째 문서 생성의 **GetStartedDemo** 메서드에 다음 코드를 복사하여 붙여넣습니다.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

**F5** 키를 눌러 응용 프로그램을 실행합니다.

축하합니다. DocumentDB 컬렉션에 대한 쿼리가 성공적으로 수행되었습니다.

다음 다이어그램에서는 만든 컬렉션에 대해 DocumentDB SQL 쿼리 구문을 호출하는 방법을 보여 주며, 마찬가지로 동일한 논리가 LINQ 쿼리에 적용됩니다.

![NoSQL에서 C# 콘솔 응용 프로그램을 만들기 위해 사용한 쿼리의 의미와 범위를 보여 주는 다이어그램](./media/documentdb-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

DocumentDB 쿼리는 이미 단일 컬렉션으로 범위가 지정되었기 때문에 [FROM](documentdb-sql-query.md#FromClause) 키워드는 쿼리에서 선택 사항입니다. 따라서 "FROM Families f"를 "FROM root r" 또는 선택한 다른 변수 이름으로 교체할 수 있습니다. DocumentDB는 패밀리, 루트 또는 선택한 변수 이름이 기본적으로 현재 컬렉션을 참조하는 것으로 유추합니다.

## <a name="a-idreplacedocumentastep-8-replace-json-document"></a><a id="ReplaceDocument"></a>8단계: JSON 문서 바꾸기
DocumentDB는 JSON 문서 바꾸기를 지원합니다.  

**ExecuteSimpleQuery** 메서드에 **ReplaceFamilyDocument** 메서드를 복사하여 붙여넣습니다.

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
        try
        {
            await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
            this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }

쿼리 실행의 **GetStartedDemo** 메서드에 다음 코드를 복사하여 붙여넣습니다. 문서를 바꾼 후에, 변경된 문서를 표시하도록 같은 쿼리가 다시 실행됩니다.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

**F5** 키를 눌러 응용 프로그램을 실행합니다.

축하합니다. DocumentDB 문서가 성공적으로 대체되었습니다.

## <a name="a-iddeletedocumentastep-9-delete-json-document"></a><a id="DeleteDocument"></a>9단계: JSON 문서 삭제
DocumentDB는 JSON 문서 삭제를 지원합니다.  

**ReplaceFamilyDocument** 메서드에 **DeleteFamilyDocument** 메서드를 복사하여 붙여넣습니다.

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
        try
        {
            await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
            Console.WriteLine("Deleted Family {0}", documentName);
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }

두 번째 쿼리 실행의 **GetStartedDemo** 메서드에 다음 코드를 복사하여 붙여넣습니다.

    await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

**F5** 키를 눌러 응용 프로그램을 실행합니다.

축하합니다. DocumentDB 문서가 성공적으로 삭제되었습니다.

## <a name="a-iddeletedatabaseastep-10-delete-the-database"></a><a id="DeleteDatabase"></a>10단계: 데이터베이스 삭제
만든 데이터베이스를 삭제하면 데이터베이스와 모든 자식 리소스(컬렉션, 문서 등)가 제거됩니다.

전체 데이터베이스와 모든 자식 리소스를 삭제하기 위해 문서 삭제의 **GetStartedDemo** 메서드에 다음 코드를 복사하여 붙여넣습니다.

    this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

    await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));

**F5** 키를 눌러 응용 프로그램을 실행합니다.

축하합니다. DocumentDB 데이터베이스가 성공적으로 삭제되었습니다.

## <a name="a-idrunastep-11-run-your-c-console-application-all-together"></a><a id="Run"></a>11단계: C# 콘솔 응용 프로그램 모두 함께 실행
디버그 모드에서 응용 프로그램을 빌드하려면 Visual Studio에서 F5 키를 누릅니다.

시작한 앱의 출력이 표시됩니다. 출력은 추가한 쿼리 결과를 보여 주며, 아래 예제 텍스트와 일치해야 합니다.

    Created FamilyDB_oa
    Press any key to continue ...
    Created FamilyCollection_oa
    Press any key to continue ...
    Created Family Andersen.1
    Press any key to continue ...
    Created Family Wakefield.7
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Replaced Family Andersen.1
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Deleted Family Andersen.1
    End of demo, press any key to exit.

축하합니다. 이 NoSQL 자습서를 완료했으며 실행되는 C# 콘솔 응용 프로그램이 셩겼습니다.

## <a name="a-idgetsolutiona-get-the-complete-nosql-tutorial-solution"></a><a id="GetSolution"></a> 전체 NoSQL 자습서 솔루션 다운로드
이 문서의 모든 샘플을 포함하는 GetStarted 솔루션을 빌드하려면 다음이 필요합니다.

* 활성 Azure 계정. 계정이 없는 경우 [무료 계정](https://azure.microsoft.com/free/)에 등록할 수 있습니다.
* [DocumentDB 계정][documentdb-create-account].
* GitHub에서 제공하는 [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) 솔루션

Visual Studio에서 DocumentDB .NET Core SDK에 대한 참조를 복원하려면 솔루션 탐색기에서 **GetStarted** 솔루션을 마우스 오른쪽 단추로 클릭한 다음 **NuGet 패키지 복원 사용**을 클릭합니다. 다음으로, Program.cs 파일에서 EndpointUrl 및 AuthorizationKey 값을 [DocumentDB 계정에 연결](#Connect)에 설명된 대로 업데이트합니다.

## <a name="next-steps"></a>다음 단계
* 보다 복잡한 ASP.NET MVC NoSQL 자습서가 필요하신가요? [DocumentDB를 사용하여 ASP.NET MVC로 웹 응용 프로그램 빌드](documentdb-dotnet-application.md)를 참조하세요.
* DocumentDB를 사용하여 규모 및 성능 테스트를 수행하려고 합니다. [Azure DocumentDB를 사용한 성능 및 규모 테스트](documentdb-performance-testing.md)
* [DocumentDB 계정 모니터링](documentdb-monitor-accounts.md)방법에 대해 자세히 알아봅니다.
* [쿼리 실습](https://www.documentdb.com/sql/demo)의 샘플 데이터 집합에 대해 쿼리를 실행합니다.
* [DocumentDB 설명서](https://azure.microsoft.com/documentation/services/documentdb/)페이지의 개발 섹션에서 프로그래밍 모델에 대해 자세히 알아봅니다.

[documentdb-create-account]: documentdb-create-account.md
[keys]: media/documentdb-dotnetcore-get-started/nosql-tutorial-keys.png

