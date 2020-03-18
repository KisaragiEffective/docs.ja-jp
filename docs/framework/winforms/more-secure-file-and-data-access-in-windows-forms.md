---
title: ファイルとデータへのより安全なアクセス
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [Windows Forms], file access
- registry [Windows Forms]
- data access [Windows Forms]
- database access (Windows Forms security)
- data [Windows Forms], secure access
- file access [Windows Forms]
- security [Windows Forms], data access
ms.assetid: 3cd3e55b-2f5e-40dd-835d-f50f7ce08967
ms.openlocfilehash: 49ba1919f68f35e9d72b012540b785e05c307c39
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743752"
---
# <a name="more-secure-file-and-data-access-in-windows-forms"></a>Windows フォームにおけるファイルおよびデータへのより安全なアクセス
.NET Framework は、リソースとデータを保護するためにアクセス許可を使用します。 アプリケーションでデータを読み取りまたは書き込みできる場所は、アプリケーションに付与されるアクセス許可に依存します。 部分信頼環境でアプリケーションを実行すると、データにアクセス許可がないか、またはデータにアクセスする方法を変更しなければならない可能性があります。  
  
 セキュリティの制限が発生した場合、アクセス許可をアサートする (アプリケーションに付与されていると仮定した場合)、または部分信頼で動作するよう作成されたバージョンの機能を使用するという 2 つのオプションがあります。 次のセクションでは、部分信頼環境で実行されているアプリケーションからファイル、データベース、およびレジストリ アクセスを操作する方法について説明します。  
  
> [!NOTE]
> 既定では、ClickOnce 配置を生成するツールは、これらの展開を実行するコンピューターから完全信頼を要求するように既定で設定されています。 部分信頼でを実行するという追加のセキュリティ上の利点が必要な場合は、Visual Studio またはいずれかの Windows SDK ツール (Mage.exe または Mageui.exe) でこの既定値を変更する必要があります。 Windows フォームセキュリティの詳細と、アプリケーションの適切な信頼レベルを決定する方法については、「 [Windows フォームの概要」の「セキュリティ](security-in-windows-forms-overview.md)」を参照してください。  
  
## <a name="file-access"></a>ファイル アクセス  
 <xref:System.Security.Permissions.FileIOPermission> クラスは、.NET Framework 内のファイルとフォルダーへのアクセスを制御します。 既定では、セキュリティ システムは、ローカルのイントラネットやインターネット ゾーンなどの部分信頼環境に <xref:System.Security.Permissions.FileIOPermission> を付与しません。 ただし、ファイルへのアクセスを必要とするアプリケーションは、アプリケーションの設計を変更するかファイルにアクセスする別の方法を使用すると、引き続きこれらの環境で機能することが可能です。 既定では、ローカル イントラネット ゾーンに、同じサイトと同じディレクトリにアクセスして、その起点のサイトに接続し、インストール ディレクトリから読み取るための権限が付与されます。 既定では、インターネット ゾーンは、起点のサイトに接続する権限のみが付与されます。  
  
### <a name="user-specified-files"></a>ユーザー指定のファイル  
 ファイルへのアクセス許可を持たないときに処理する方法の 1 つは、<xref:System.Windows.Forms.OpenFileDialog> クラスまたは <xref:System.Windows.Forms.SaveFileDialog> クラスを使用して、特定のファイル情報の入力をユーザーに求める方法です。 このユーザーの操作では、アプリケーションが悪意を持ってプライベート ファイルを読み込んだり、重要なファイルを上書きしたりできないことを、ある程度保証できます。 <xref:System.Windows.Forms.OpenFileDialog.OpenFile%2A> メソッドと <xref:System.Windows.Forms.SaveFileDialog.OpenFile%2A> メソッドは、ユーザーが指定したファイルのファイル ストリームを開くことで、ファイルの読み取りアクセスと書き込みアクセスを提供します。 メソッドは、ファイルのパスを隠すことで、ユーザーのファイルの保護にも役立ちます。  
  
> [!NOTE]
> これらのアクセス許可は、アプリケーションが、インターネット ゾーン内かイントラネット ゾーン内かに応じて異なります。 インターネット ゾーンのアプリケーションは <xref:System.Windows.Forms.OpenFileDialog> のみを使用できますが、イントラネット アプリケーションは、無制限のファイル ダイアログのアクセス許可を持っています。  
  
 <xref:System.Security.Permissions.FileDialogPermission> クラスは、アプリケーションで使用できるファイル ダイアログ ボックスの種類を指定します。 次の表は、各 <xref:System.Windows.Forms.FileDialog> クラスを使用するために必要な値を示しています。  
  
|クラス|必要なアクセス権の値|  
|-----------|---------------------------|  
|<xref:System.Windows.Forms.OpenFileDialog>|<xref:System.Security.Permissions.FileDialogPermissionAccess.Open>|  
|<xref:System.Windows.Forms.SaveFileDialog>|<xref:System.Security.Permissions.FileDialogPermissionAccess.Save>|  
  
> [!NOTE]
> <xref:System.Windows.Forms.OpenFileDialog.OpenFile%2A> メソッドが実際に呼び出されるまでは、特定のアクセス許可は要求されません。  
  
 ファイル ダイアログ ボックスを表示するためのアクセス許可は、<xref:System.Windows.Forms.FileDialog>、<xref:System.Windows.Forms.OpenFileDialog>、および <xref:System.Windows.Forms.SaveFileDialog> の各クラスのすべてのメンバーに対するフルアクセスをアプリケーションに付与しません。 各メソッドを呼び出すために必要なアクセス許可については、.NET Framework クラスライブラリのドキュメントで、そのメソッドのリファレンストピックを参照してください。  
  
 次のコード例では、<xref:System.Windows.Forms.OpenFileDialog.OpenFile%2A> メソッドを使用して、ユーザーが指定したファイルを <xref:System.Windows.Forms.RichTextBox> コントロールに開きます。 この例では、<xref:System.Security.Permissions.FileDialogPermission> および関連付けられた <xref:System.Security.Permissions.FileDialogPermissionAttribute.Open%2A> 列挙値が必要です。 例では、保存機能を無効にする必要があるかどうかを判断するために、<xref:System.Security.SecurityException> を処理する方法を示しています。 この例では、<xref:System.Windows.Forms.Form> が <xref:System.Windows.Forms.Button> という名前の `ButtonOpen` コントロールと、<xref:System.Windows.Forms.RichTextBox> という名前の `RtfBoxMain` コントロールを必要とします。  
  
> [!NOTE]
> 保存機能のプログラミング ロジックは、この例では表示されません。  
  
```vb  
Private Sub ButtonOpen_Click(ByVal sender As System.Object, _  
    ByVal e As System.EventArgs) Handles ButtonOpen.Click   
  
    Dim editingFileName as String = ""  
    Dim saveAllowed As Boolean = True  
  
    ' Displays the OpenFileDialog.  
    If (OpenFileDialog1.ShowDialog() = DialogResult.OK) Then  
        Dim userStream as System.IO.Stream  
        Try   
            ' Opens the file stream for the file selected by the user.  
            userStream =OpenFileDialog1.OpenFile()   
            Me.RtfBoxMain.LoadFile(userStream, _  
                RichTextBoxStreamType.PlainText)  
        Finally  
            userStream.Close()  
        End Try  
  
        ' Tries to get the file name selected by the user.  
        ' Failure means that the application does not have  
        ' unrestricted permission to the file.  
        Try   
            editingFileName = OpenFileDialog1.FileName  
        Catch ex As Exception  
            If TypeOf ex Is System.Security.SecurityException Then   
                ' The application does not have unrestricted permission   
                ' to the file so the save feature will be disabled.  
                saveAllowed = False   
            Else   
                Throw ex  
            End If  
        End Try  
    End If  
End Sub  
```  
  
```csharp  
private void ButtonOpen_Click(object sender, System.EventArgs e)   
{  
    String editingFileName = "";  
    Boolean saveAllowed = true;  
  
    // Displays the OpenFileDialog.  
    if (openFileDialog1.ShowDialog() == DialogResult.OK)   
    {  
        // Opens the file stream for the file selected by the user.  
        using (System.IO.Stream userStream = openFileDialog1.OpenFile())   
        {  
            this.RtfBoxMain.LoadFile(userStream,  
                RichTextBoxStreamType.PlainText);  
            userStream.Close();  
        }  
  
        // Tries to get the file name selected by the user.  
        // Failure means that the application does not have  
        // unrestricted permission to the file.  
        try   
        {  
            editingFileName = openFileDialog1.FileName;  
        }   
        catch (Exception ex)   
        {  
            if (ex is System.Security.SecurityException)   
            {  
                // The application does not have unrestricted permission   
                // to the file so the save feature will be disabled.  
                saveAllowed = false;   
            }   
            else   
            {  
                throw ex;  
            }  
        }  
    }  
}  
```  
  
> [!NOTE]
> Visual C# では、イベント ハンドラーを有効にするコードを必ず追加してください。 前の例でのコードを使用することで、次のコードではイベント ハンドラー `this.ButtonOpen.Click += newSystem.Windows.Forms.EventHandler(this.ButtonOpen_Click);` を有効にする方法を示します。  
  
### <a name="other-files"></a>他のファイル数  
 場合によって、アプリケーション設定を保存しなければならないという場合など、ユーザーが指定しないファイルへの読み取りまたは書き込みを実行する必要があります。 ローカルのイントラネット ゾーンとインターネット ゾーンでは、アプリケーションにローカル ファイルにデータを格納するためのアクセス許可がありません。 ただし、アプリケーションが分離ストレージにデータを格納できるようにします。 分離ストレージは (特定の記憶場所ではなく) 抽象的なデータ コンパートメントであり、データが保存される実際のディレクトリの場所を含む、ストアと呼ばれる 1 つ以上の分離ストレージ ファイルが含まれます。 <xref:System.Security.Permissions.FileIOPermission> のようなファイルのアクセス許可は必要なく、代わりに <xref:System.Security.Permissions.IsolatedStoragePermission> クラスが分離ストレージのアクセス許可を制御します。 既定では、ローカル イントラネット ゾーンとインターネット ゾーンで実行されているアプリケーションが、分離ストレージを使用してデータを格納できます。ただし、ディスク クォータのように設定が変わることがあります。 分離ストレージの詳細については、「[分離ストレージ](../../standard/io/isolated-storage.md)」を参照してください。  
  
 次の例では、分離ストレージを使用して、ストアにあるファイルにデータを書き込みます。 この例では、<xref:System.Security.Permissions.IsolatedStorageFilePermission> および <xref:System.Security.Permissions.IsolatedStorageContainment.DomainIsolationByUser> 列挙値が必要です。 例では、分離ストレージ内のファイルに対する、<xref:System.Windows.Forms.Button> コントロールの特定のプロパティ値の読み取りと書き込みを示しています。 `Read` 関数はアプリケーションの起動後に呼び出され、`Write` 関数はアプリケーションの終了前に呼び出されます。 この例では、`Read` および `Write` 関数が `MainButton`という名前の <xref:System.Windows.Forms.Button> コントロールを含む <xref:System.Windows.Forms.Form> のメンバーとして存在する必要があります。  
  
```vb  
' Reads the button options from the isolated storage. Uses Default values   
' for the button if the options file does not exist.  
Public Sub Read()   
    Dim isoStore As System.IO.IsolatedStorage.IsolatedStorageFile = _  
        System.IO.IsolatedStorage.IsolatedStorageFile. _   
        GetUserStoreForDomain()  
  
    Dim filename As String = "options.txt"  
    Try  
        ' Checks to see if the options.txt file exists.  
        If (isoStore.GetFileNames(filename).GetLength(0) <> 0) Then  
  
            ' Opens the file because it exists.  
            Dim isos As New System.IO.IsolatedStorage.IsolatedStorageFileStream _   
                 (filename, IO.FileMode.Open,isoStore)  
            Dim reader as System.IO.StreamReader  
            Try   
                reader = new System.IO.StreamReader(isos)  
  
                ' Reads the values stored.  
                Dim converter As System.ComponentModel.TypeConverter  
                converter = System.ComponentModel.TypeDescriptor.GetConverter _   
                    (GetType(Color))  
  
                Me.MainButton.BackColor = _   
                        CType(converter.ConvertFromString _   
                         (reader.ReadLine()), Color)  
                me.MainButton.ForeColor = _  
                        CType(converter.ConvertFromString _   
                         (reader.ReadLine()), Color)  
  
                converter = System.ComponentModel.TypeDescriptor.GetConverter _   
                    (GetType(Font))  
                me.MainButton.Font = _  
                        CType(converter.ConvertFromString _   
                         (reader.ReadLine()), Font)  
  
            Catch ex As Exception  
                Debug.WriteLine("Cannot read options " + _  
                    ex.ToString())  
            Finally  
                reader.Close()  
            End Try  
        End If  
  
    Catch ex As Exception  
        Debug.WriteLine("Cannot read options " + ex.ToString())  
    End Try  
End Sub  
  
' Writes the button options to the isolated storage.  
Public Sub Write()   
    Dim isoStore As System.IO.IsolatedStorage.IsolatedStorageFile = _  
        System.IO.IsolatedStorage.IsolatedStorageFile. _   
        GetUserStoreForDomain()  
  
    Dim filename As String = "options.txt"  
    Try   
        ' Checks if the file exists, and if it does, tries to delete it.  
        If (isoStore.GetFileNames(filename).GetLength(0) <> 0) Then  
            isoStore.DeleteFile(filename)  
        End If  
    Catch ex As Exception  
        Debug.WriteLine("Cannot delete file " + ex.ToString())  
    End Try  
  
    ' Creates the options.txt file and writes the button options to it.  
    Dim writer as System.IO.StreamWriter  
    Try   
        Dim isos As New System.IO.IsolatedStorage.IsolatedStorageFileStream _   
             (filename, IO.FileMode.CreateNew, isoStore)  
  
        writer = New System.IO.StreamWriter(isos)  
        Dim converter As System.ComponentModel.TypeConverter  
  
        converter = System.ComponentModel.TypeDescriptor.GetConverter _   
           (GetType(Color))  
        writer.WriteLine(converter.ConvertToString( _  
            Me.MainButton.BackColor))  
        writer.WriteLine(converter.ConvertToString( _  
            Me.MainButton.ForeColor))  
  
        converter = System.ComponentModel TypeDescriptor.GetConverter _   
           (GetType(Font))  
        writer.WriteLine(converter.ConvertToString( _  
            Me.MainButton.Font))  
  
    Catch ex as Exception  
        Debug.WriteLine("Cannot write options " + ex.ToString())  
  
    Finally  
        writer.Close()  
    End Try  
End Sub  
```  
  
```csharp  
// Reads the button options from the isolated storage. Uses default values   
// for the button if the options file does not exist.  
public void Read()   
{  
    System.IO.IsolatedStorage.IsolatedStorageFile isoStore =   
        System.IO.IsolatedStorage.IsolatedStorageFile.  
        GetUserStoreForDomain();  
  
    string filename = "options.txt";  
    try  
    {  
        // Checks to see if the options.txt file exists.  
        if (isoStore.GetFileNames(filename).GetLength(0) != 0)   
        {  
            // Opens the file because it exists.  
            System.IO.IsolatedStorage.IsolatedStorageFileStream isos =   
                new System.IO.IsolatedStorage.IsolatedStorageFileStream  
                    (filename, System.IO.FileMode.Open,isoStore);  
            System.IO.StreamReader reader = null;  
            try   
            {  
                reader = new System.IO.StreamReader(isos);  
  
                // Reads the values stored.  
                TypeConverter converter ;  
                converter = TypeDescriptor.GetConverter(typeof(Color));  
  
                this.MainButton.BackColor =   
                 (Color)(converter.ConvertFromString(reader.ReadLine()));  
                this.MainButton.ForeColor =   
                 (Color)(converter.ConvertFromString(reader.ReadLine()));  
  
                converter = TypeDescriptor.GetConverter(typeof(Font));  
                this.MainButton.Font =   
                  (Font)(converter.ConvertFromString(reader.ReadLine()));  
            }  
            catch (Exception ex)  
            {   
                System.Diagnostics.Debug.WriteLine  
                     ("Cannot read options " + ex.ToString());  
            }  
            finally  
            {  
                reader.Close();  
            }  
        }  
    }   
    catch (Exception ex)   
    {  
        System.Diagnostics.Debug.WriteLine  
            ("Cannot read options " + ex.ToString());  
    }  
}  
  
// Writes the button options to the isolated storage.  
public void Write()   
{  
    System.IO.IsolatedStorage.IsolatedStorageFile isoStore =   
        System.IO.IsolatedStorage.IsolatedStorageFile.  
        GetUserStoreForDomain();  
  
    string filename = "options.txt";  
    try   
    {  
        // Checks if the file exists and, if it does, tries to delete it.  
        if (isoStore.GetFileNames(filename).GetLength(0) != 0)   
        {  
            isoStore.DeleteFile(filename);  
        }  
    }  
    catch (Exception ex)   
    {  
        System.Diagnostics.Debug.WriteLine  
            ("Cannot delete file " + ex.ToString());  
    }  
  
    // Creates the options file and writes the button options to it.  
    System.IO.StreamWriter writer = null;  
    try   
    {  
        System.IO.IsolatedStorage.IsolatedStorageFileStream isos = new   
            System.IO.IsolatedStorage.IsolatedStorageFileStream(filename,   
            System.IO.FileMode.CreateNew,isoStore);  
  
        writer = new System.IO.StreamWriter(isos);  
        TypeConverter converter ;  
  
        converter = TypeDescriptor.GetConverter(typeof(Color));  
        writer.WriteLine(converter.ConvertToString(  
            this.MainButton.BackColor));  
        writer.WriteLine(converter.ConvertToString(  
            this.MainButton.ForeColor));  
  
        converter = TypeDescriptor.GetConverter(typeof(Font));  
        writer.WriteLine(converter.ConvertToString(  
            this.MainButton.Font));  
  
    }  
    catch (Exception ex)  
    {   
        System.Diagnostics.Debug.WriteLine  
           ("Cannot write options " + ex.ToString());  
    }  
    finally  
    {  
        writer.Close();  
    }  
}  
```  
  
## <a name="database-access"></a>データベース アクセス  
 データベースにアクセスするために必要なアクセス許可は、データベース プロバイダーに応じて異なります。ただし、適切なアクセス許可で実行されているアプリケーションのみがデータ接続を使用してデータベースにアクセスできます。 データベースへのアクセスに必要な権限の詳細については、「[コードアクセスセキュリティと ADO.NET](../data/adonet/code-access-security.md)」を参照してください。  
  
 アプリケーションを部分信頼で実行するためにデータベースに直接アクセスできない場合は、データにアクセスする別の方法として Web サービスを使用することができます。 Web サービスは、ネットワーク経由でプログラムでアクセスできるソフトウェアです。 Web サービスを使用すると、アプリケーションはコード グループのゾーンにまたがるデータを共有できます。 既定では、ローカル イントラネット ゾーンとインターネット ゾーンのアプリケーションに、元のサイトへのアクセス権が付与され、同じサーバーにホストされる Web サービスを呼び出すことができます。 詳細については、「 [ASP.NET AJAX の Web サービス](https://docs.microsoft.com/previous-versions/aspnet/bb398785(v=vs.100))」または「 [Windows Communication Foundation](../wcf/index.md)」を参照してください。  
  
## <a name="registry-access"></a>レジストリへのアクセス  
 <xref:System.Security.Permissions.RegistryPermission> クラスは、オペレーティング システムのレジストリへのアクセスを制御します。 既定では、ローカルで実行されているアプリケーションのみが、レジストリにアクセスできます。  <xref:System.Security.Permissions.RegistryPermission> はレジストリにアクセスしようとする権利のみをアプリケーションに付与します。オペレーティング システムは、引き続きレジストリのセキュリティを強制するため、アクセスが成功するかどうかは保証されません。  
  
 部分信頼の下のレジストリにアクセスすることはできないため、データを格納するその他のメソッドを検索する必要がある場合があります。 アプリケーション設定を保存する場合、レジストリの代わりに分離ストレージを使用します。 また、分離ストレージを使用して、その他のアプリケーション固有のファイルを格納することもできます。 既定では、アプリケーションの発生元のサイトへのアクセスがアプリケーションに付与されているため、サーバーまたは元のサイトに関するグローバルなアプリケーションの情報を格納することもできます。  
  
## <a name="see-also"></a>参照

- [Windows フォームでのより安全な印刷](more-secure-printing-in-windows-forms.md)
- [Windows フォームのセキュリティに関するその他の考慮事項](additional-security-considerations-in-windows-forms.md)
- [Windows フォームのセキュリティの概要](security-in-windows-forms-overview.md)
- [Windows フォームのセキュリティ](windows-forms-security.md)
- [Mage.exe (マニフェストの生成および編集ツール)](../tools/mage-exe-manifest-generation-and-editing-tool.md)
- [MageUI.exe (マニフェスト生成および編集ツールのグラフィカル クライアント)](../tools/mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)
