BDD4TC
=============

BDD-like library for TestComplete (Biblioteca BDD-like para o TestComplete)


Contributing
------------

Want to contribute? Great! Feel free to improve the library.


Requirements
-----------

    TestComplete 9 or superior



Limitations
-----------

    It runs only on vbscript


Feature/Scenario file
-----------

The scenario file is a plain text file with the .txt extension. It follows the given/when/then format. The method parameters must be quoted. Example (in portuguese filename: Simular financiamento de imóvel residencial.txt):

	Cenário: Simular financiamento de imóvel residencial
	Dado que
 	  O imóvel é do tipo residencial
	Quando
 	  O valor do imóvel é igual "100.000,00"
 	  O valor da entrada é igual a "20.000,00"
 	  O prazo de financiamento é igual a "12" meses
 	  A data de nascimento é igual a "18/12/1977"
 	  A renda líquida mensal é igual a "7.000,00"
 	  A simulação é executada
	Então
  	  O valor da primeira parcela deve ser "7.319,74"
  	  O valor da última parcela deve ser "6.752,30"


How it works
-----------

*  On TestComplete run BDD4TC.RunFromFile(filepath) or RunFromFolder(foldername) method.
*  The BDD4TC library parses the file name, removes the special characters and substitutes the whitespaces to underscores. Example ("Simular financiamento de imóvel residencial.txt" is parsed to "simular_financiamento_de_imovel_residencial.txt".
*  The BDD4TC library look for a unit named "simular_financiamento_de_imovel_residencial". The unit name must be the file named parsed without the extension.
*  The BDD4TC library parses the lines inside the text file. For each line, it looks for a method implementantion under the unit. This method implementation is the fixture that runs the step definition.
*  The unit must have a method name equal the text line (parsed). For example: the text line says "A renda líquida mensal é igual a "7.000,00" then BDD4TC looks for a method named "sub a_renda_liquida_mensal_e_igual_a_x(p)". The parameter "7.000,00" is changed to x. All parameters are changed to x. Another example: the text line says "A simulação é executada" then BDD4TC looks for a method named "sub a_simulacao_e_executada(p)".
*  All methods implementations must have a parameter (p). It contains the original text line. So inside the methods you can freely parses (via regex) the information you need.


More Information
-----------

[Lecture about BDD and TestComplete](http://www.slideshare.net/cristianocaetano/test-day-2012)


Example
-----------

(in portuguese filename: Simular financiamento de imóvel residencial.txt):

	Cenário: Simular financiamento de imóvel residencial
	Dado que
 	  O imóvel é do tipo residencial
	Quando
 	  O valor do imóvel é igual "100.000,00"
 	  O valor da entrada é igual a "20.000,00"
 	  O prazo de financiamento é igual a "12" meses
 	  A data de nascimento é igual a "18/12/1977"
 	  A renda líquida mensal é igual a "7.000,00"
 	  A simulação é executada
	Então
  	  O valor da primeira parcela deve ser "7.319,74"
  	  O valor da última parcela deve ser "6.752,30"

(Unit name: simulador_de_credito_imobiliario_de_imovel_residencial)

        sub o_imovel_e_do_tipo_residencial(p)
          Log.LockEvents
    
          Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Table(0).Cell(0, 1).Table(0).Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 1).Form("frmSimula").Table("Table1").Cell(0, 0).Table("Table2").Cell(4, 1).Table("Table3").Cell(5, 0).RadioButton("rdoTipoImovelRes").Click
          Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Wait
        end sub
        
        sub o_valor_do_imovel_e_igual_x(p)
          Log.LockEvents
          Set regEx = New RegExp
          regEx.Global = True
          regEx.Pattern = chr(34) & "(.+?)" & chr(34)    
          Set Matches = regEx.Execute(p)
          ValorImovel = aqString.Unquote(Matches(0).Value)
          
          Call Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Table(0).Cell(0, 1).Table(0).Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 1).Form("frmSimula").Table("Table1").Cell(0, 0).Table("Table2").Cell(4, 1).Table("Table3").Cell(9, 0).Textbox("txtValorImovel").SetText(ValorImovel)
        end sub
        
        
        sub o_valor_da_entrada_e_igual_a_x(p)
          Log.LockEvents
          Set regEx = New RegExp
          regEx.Global = True
          regEx.Pattern = chr(34) & "(.+?)" & chr(34)    
          Set Matches = regEx.Execute(p)
          ValorEntrada = aqString.Unquote(Matches(0).Value)
        
          Call Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Table(0).Cell(0, 1).Table(0).Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 1).Form("frmSimula").Table("Table1").Cell(0, 0).Table("Table2").Cell(4, 1).Table("Table3").Cell(12, 0).Textbox("txtValorEntrada").SetText(ValorEntrada)
        end sub  
        
        sub o_prazo_de_financiamento_e_igual_a_x_meses(p)
          Log.LockEvents
          Set regEx = New RegExp
          regEx.Global = True
          regEx.Pattern = chr(34) & "(.+?)" & chr(34)    
          Set Matches = regEx.Execute(p)
          PrazoFinanciamento = aqString.Unquote(Matches(0).Value)
        
          Call Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Table(0).Cell(0, 1).Table(0).Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 1).Form("frmSimula").Table("Table1").Cell(0, 0).Table("Table2").Cell(4, 1).Table("Table3").Cell(14, 0).Textbox("txtPrazo").SetText(PrazoFinanciamento)
        end sub
        
        sub a_data_de_nascimento_e_igual_a_x(p)
          Log.LockEvents
          Set regEx = New RegExp
          regEx.Global = True
          regEx.Pattern = chr(34) & "(.+?)" & chr(34)    
          Set Matches = regEx.Execute(p)
          DataNascimento = aqString.Unquote(Matches(0).Value)
          
          Dia = IntToStr(aqDateTime.GetDay(StrToDate(DataNascimento)))
          Mes = IntToStr(aqDateTime.GetMonth(StrToDate(DataNascimento)))
          Ano = IntToStr(aqDateTime.GetYear(StrToDate(DataNascimento)))
        
          Call Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Table(0).Cell(0, 1).Table(0).Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 1).Form("frmSimula").Table("Table1").Cell(0, 0).Table("Table2").Cell(6, 1).Table("Table4").Cell(3, 0).Textbox("txtDiaNascProp1").Keys(Dia)
          Call Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Table(0).Cell(0, 1).Table(0).Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 1).Form("frmSimula").Table("Table1").Cell(0, 0).Table("Table2").Cell(6, 1).Table("Table4").Cell(3, 0).Textbox("txtMesNascProp1").Keys(Mes)
          Call Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Table(0).Cell(0, 1).Table(0).Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 1).Form("frmSimula").Table("Table1").Cell(0, 0).Table("Table2").Cell(6, 1).Table("Table4").Cell(3, 0).Textbox("txtAnoNascProp1").Keys(Ano)
        end sub
        
        sub a_renda_liquida_mensal_e_igual_a_x(p)
          Log.LockEvents
          Set regEx = New RegExp
          regEx.Global = True
          regEx.Pattern = chr(34) & "(.+?)" & chr(34)    
          Set Matches = regEx.Execute(p)
          RendaLiquida = aqString.Unquote(Matches(0).Value)
          
          Call Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Table(0).Cell(0, 1).Table(0).Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 1).Form("frmSimula").Table("Table1").Cell(0, 0).Table("Table2").Cell(6, 1).Table("Table4").Cell(6, 0).Textbox("txtValorRendaProp1").SetText(RendaLiquida)
        end sub
        
        sub a_simulacao_e_executada(p)
          Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Table(0).Cell(0, 1).Table(0).Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 1).Form("frmSimula").Table("Table1").Cell(0, 0).ImageButton("btSimular").Click
          Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/index.aspx*").Wait
        end sub
        
        sub o_valor_da_primeira_parcela_deve_ser_x(p)
          Log.LockEvents
          Set regEx = New RegExp
          regEx.Global = True
          regEx.Pattern = chr(34) & "(.+?)" & chr(34)    
          Set Matches = regEx.Execute(p)
          PrimeiraParcela = aqString.Unquote(Matches(0).Value)
          
          Call aqObject.CheckProperty(Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/simulador.aspx?IMOB_TipoBKL=&ident_bkl=pre").Table("Table2").Cell(0, 1).Table("Table3").Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 2).Form("frmSimula").Table("Table2").Cell(8, 0).Table("dtgPrazos").Cell(1, 5), "innerText", cmpEqual, PrimeiraParcela, False)
        end sub
        
        sub o_valor_da_ultima_parcela_deve_ser_x(p)
          Log.LockEvents
          Set regEx = New RegExp
          regEx.Global = True
          regEx.Pattern = chr(34) & "(.+?)" & chr(34)    
          Set Matches = regEx.Execute(p)
          UltimaParcela = aqString.Unquote(Matches(0).Value)
        
          Call aqObject.CheckProperty(Sys.Browser.Page("https://ww3.itau.com.br/imobline/pre/simuladores_new/fichaProposta/simulador.aspx?IMOB_TipoBKL=&ident_bkl=pre").Table("Table2").Cell(0, 1).Table("Table3").Cell(0, 2).Table("tab_bordaPrincipal").Cell(0, 2).Form("frmSimula").Table("Table2").Cell(8, 0).Table("dtgPrazos").Cell(1, 6), "innerText", cmpEqual, UltimaParcela, False)
        end sub


