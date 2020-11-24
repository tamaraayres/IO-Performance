unit Unit1;

interface

uses
  Windows, Messages, SysUtils, Variants, Classes, Graphics, Controls, Forms,
  Dialogs, StdCtrls, Mask, ExtCtrls, DateUtils, ShellApi, ComCtrls;

type
  TForm1 = class(TForm)
    PanNumeroLinhas: TPanel;
    LblNumeroLinhas: TLabel;
    EdtNumeroLinhas: TMaskEdit;
    BtnGerarArquivos: TButton;
    PanNumeroArquivos: TPanel;
    EdtNumeroArquivos: TMaskEdit;
    PanGeral: TPanel;
    LblNumeroArquivos: TLabel;
    ProgressBar1: TProgressBar;
    procedure BtnGerarArquivosClick(Sender: TObject);
    procedure EdtNumeroLinhasKeyPress(Sender: TObject; var Key: Char);
  private
    { Private declarations }
  public
    { Public declarations }
    procedure GeraArquivos(qtde: String);
    procedure GeraArquivo(dir, arqName, linhas: String);
  end;

var
  Form1: TForm1;

implementation

{$R *.dfm}

procedure TForm1.GeraArquivos(qtde: String);
var i, n: integer;
    inicio : TDateTime;
    dir : String;
begin
  try
    if ((Trim(EdtNumeroArquivos.Text) = '') or (Trim(EdtNumeroLinhas.Text) = '')) then
     begin
         ShowMessage('Informe o número de arquivos e o número de linhas!');
         exit;
     end;

    inicio := Now;

    n := StrToInt(qtde); 

    ProgressBar1.Max := n;

    dir := GetCurrentDir + '\' + FormatDateTime('dd-mm-yyyy_hh-MM-ss', inicio);
    CreateDir(dir);

    for i:=1 to n do
    begin
       GeraArquivo(dir, IntToStr(i), EdtNumeroLinhas.Text);

       ProgressBar1.Position := i;

       //Sleep(25);
    end;

    //ShowMessage(qtde + ' arquivo(s) de ' + EdtNumeroLinhas.Text + ' linhas criado(s) com sucesso em ' + IntToStr(DateUtils.MilliSecondsBetween(Now, inicio)) + ' milisegundos. '+ #13+
    //            'Diretório: ' + dir);

    ProgressBar1.Position:= 0;

    if MessageDlg(qtde + ' arquivo(s) de ' + EdtNumeroLinhas.Text + ' linhas criado(s) com sucesso em ' + IntToStr(DateUtils.MilliSecondsBetween(Now, inicio)) + ' milisegundos. '+ #13+
                  'Diretório: ' + dir + #13 + #13 + 'Deseja abrir o diretório?', mtConfirmation,
                  [mbYes, mbNo], 0) = mrYes then
    Begin
    ShellExecute(Application.Handle,
               PChar('explore'),
               PChar(dir),
               nil,
               nil,
               SW_SHOWNORMAL);
    End;
  except on e : exception do
    ShowMessage('Erro: ' + e.Message);
  end;
end; 

procedure TForm1.GeraArquivo(dir, arqName, linhas: String);
var arq: TextFile; 
    i, n: integer;
    nomeArq: String;
    inicioArq, fimArq: TDateTime;
begin
  try
    n := StrToInt(linhas);

    nomeArq :=  dir + '\' + arqName + '.txt';
    AssignFile(arq, nomeArq);

    Rewrite(arq,nomeArq);

    Append(arq);

    inicioArq := Now;
    for i:=1 to n do
    begin
      Writeln(arq, 'Linha ' + IntToStr(i));
    end;
    fimArq := Now;
     
    Writeln(arq, #13 + 'Inicio: ' + FormatDateTime('dd/mm/yyyy hh:MM:ss.zzz', inicioArq));
    Writeln(arq, 'Fim: ' + FormatDateTime('dd/mm/yyyy hh:MM:ss.zzz', fimArq) + #13);
    
    Write(arq, 'Total: ' + IntToStr(DateUtils.MilliSecondsBetween(fimArq, inicioArq)) + ' milisegundos');

    CloseFile(arq);
  except on e : exception do
    ShowMessage('Erro: ' + e.Message);
  end;
end; 

procedure TForm1.BtnGerarArquivosClick(Sender: TObject);
begin
   if Trim(EdtNumeroArquivos.Text) = '' then
     begin
         ShowMessage('Informe o número de arquivos e o número de linhas!');
         EdtNumeroArquivos.Focused;
     end
     else
     begin
      GeraArquivos(EdtNumeroArquivos.Text);
     end;
end;

procedure TForm1.EdtNumeroLinhasKeyPress(Sender: TObject; var Key: Char);
begin
      if (Key = #13) then
        GeraArquivos(Trim(EdtNumeroArquivos.Text));

      if not (Key in ['0'..'9',#08]) then
        key := #0;
end;

end.
