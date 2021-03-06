---
title: Практическое руководство. Запись текста в файлы с помощью StreamWriter
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], writing to
- text, writing to files
- writing to files [Visual Basic], StreamWriter
ms.assetid: 99762e57-ef46-4dcc-8959-a8f79c22f067
ms.openlocfilehash: 373f3bd07ea61263ddd81037d8cbbb06f789e599
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411581"
---
# <a name="how-to-write-text-to-files-with-a-streamwriter-in-visual-basic"></a>Практическое руководство. Запись текста в файлы с помощью StreamWriter в Visual Basic

В этом примере с помощью метода `My.Computer.FileSystem.OpenTextFileWriter` открывается объект <xref:System.IO.StreamWriter>, который используется для записи строки в текстовый файл с помощью метода <xref:System.IO.TextWriter.WriteLine%2A> класса <xref:System.IO.StreamWriter>.  
  
## <a name="example"></a>Пример  

 [!code-vb[VbFileIOWrite#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#5)]  
  
## <a name="robust-programming"></a>Отказоустойчивость  

 При следующих условиях возможно возникновение исключения:  
  
- Файл существует и является файлом только для чтения (<xref:System.IO.IOException>).  
  
- Диск заполнен (<xref:System.IO.IOException>).  
  
- Слишком длинное имя пути (<xref:System.IO.PathTooLongException>).  
  
## <a name="net-framework-security"></a>Безопасность .NET Framework  

 В этом примере создается файл (если файл отсутствует). Если приложению требуется создать файл, оно должно иметь доступ к каталогу для создания файлов (`Create`). Если файл уже существует, то приложению достаточно иметь лишь доступ для записи файлов (`Write`), т. е. меньшие привилегии. Безопаснее создавать файл во время развертывания, если это возможно, а также предоставлять доступ `Read` к отдельному файлу вместо доступа `Create` к папке.  
  
## <a name="see-also"></a>См. также раздел

- <xref:System.IO.StreamWriter>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFileWriter%2A>
- [Практическое руководство. Чтение из текстовых файлов](how-to-read-from-text-files.md)
- [Запись в файлы](writing-to-files.md)
