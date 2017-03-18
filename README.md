package KsiazkaTelefoniczna;

import java.io.File;
import java.io.IOException;

public class Ksiazka {
	
	private static Kontakt[] kontakty; 
	static File plikDanych = new File("kontakty.tel");
	
	
	public static void main(String[] args) throws Exception  {
		
		ladowanieDanych();
		
		System.out.println("Witaj Użytkowniku!");
		
		
		
		System.out.println("Podaj polecenie");
	
		boolean czyKoniec = false;
	
		do{
			System.out.print("> ");
			 String polecenie = Utils.readLine();
			
			System.out.println(polecenie);
			String[] parametryPolecenia = polecenie.split(" ",2);
			polecenie = parametryPolecenia[0];
			String parametr = "";
			if(parametryPolecenia.length>1){
				parametr = parametryPolecenia[1];
				
			}
				// interpretacja poleceń
			if("dodaj".equals(polecenie) || "add".equals(polecenie)){
				if(parametr.length()>0){
				dodajKontakt(parametr);
				} else {
					System.out.println("Nie można dodać kontaktu bez nazwy");
				}
			}else
			if("lista".equals(polecenie) || "list".equals(polecenie)){
				lista();
			}else
		
			if("pokaż".equals(polecenie)|| "show".equals(polecenie)){
				if(parametr.length()>0){
					pokazKontakt(parametr);
					} else {
						System.out.println("Nie można wyświetlić kontaktu bez nazwy!");
					}			
			}		else {
				
			if("usuń".equals(polecenie)|| "del".equals(polecenie)){
				if(parametr.length()>0){
					usunKontakt(parametr);
				} else{
					System.out.println("Nie można usunąć kontaktu bez nazwy!");
				}
				if("sortuj".equals(polecenie) || "sort".equals(polecenie)){
					sortuj();
			}		
			if("koniec".equals(polecenie)|| "end".equals(polecenie)){
				czyKoniec = true;
				
			} else{
				System.out.println("Nie ma takiego polecenia"+polecenie+"!");
				}
			}
			}
			
		} while(!czyKoniec);
		
		
		System.out.println("Zapis danych...");
		Utils.zapiszObiektDoPliku(plikDanych, kontakty);
		System.out.println("Dane zapisano");
		
			System.out.println("Żegnaj użytkowniku!");
		}
		/** sortowanie po nazwie */
	private static void sortuj() {
		
		for(int i=0; i<kontakty.length-1; i++){
			for(int j=i+1; j<kontakty.length; j++){
				String a = kontakty[i].nazwa;
				String b = kontakty[j].nazwa;
				Kontakt buf = null;
				
					if(a.compareTo(b)>0){ 
						buf = kontakty[i];
						kontakty[i] = kontakty[j];
						kontakty[j] = buf;
					}
				}
			}
		}
		
		
	private static void usunKontakt(String nazwaKontaktu) {
		Kontakt kontakt = null;
		
		for(Kontakt k : kontakty){
			if(k.nazwa.toLowerCase().equals(nazwaKontaktu.toLowerCase())){
				kontakt = k;
			}
		}
		if(kontakt==null){
			System.out.println("Nie znaleziono kontaktu do usunięcia: \""+nazwaKontaktu+"\"");
		} else{
			Kontakt[] nowa = new Kontakt[kontakty.length-1];
			int i=0;
			
			for(Kontakt k : kontakty) if(k!= kontakt){
				nowa[i] = k; 
				i++;
			
			}
			
			kontakty = nowa;
			System.out.println("Kontakt został usunięty.");
		}
		
	}
	//** Ta metoda pokazuje dane kontaktu wybranego po nazwie  @param nazwaKontaktu nazwa kontaktu do wyśietlenia */
	private static void pokazKontakt(String nazwaKontaktu) {
		Kontakt kontakt = null;
		
		for(Kontakt k : kontakty){
			if(k.nazwa.toLowerCase().equals(nazwaKontaktu.toLowerCase())){
				kontakt = k;
			}
		}
		if(kontakt==null){
			System.out.println("Nie znaleziono kontaktu o nazwie: \""+nazwaKontaktu+"\"");
		} else { // znaleziono kontakt
			System.out.println("Dane kontaktu");
			System.out.println("    Nazwa"+kontakt.nazwa);
			System.out.println("    NrTel"+kontakt.telefon);
		}
		
	}

	private static void lista() {
		System.out.println("Listowanie kontaktów:");
		
		
		
		if(kontakty.length>0){
			
			int dlNazwy=0, dlTel=0;
			for(int i=0; i<kontakty.length; i++){
				if(kontakty[i].nazwa.length()>dlNazwy){
					dlNazwy = kontakty[i].nazwa.length();
				}
				if(kontakty[i].telefon.length()>dlTel){
					dlTel = kontakty[i].telefon.length();
				}
			
			}
		for(int i=0; i<kontakty.length; i++){
			
			String format = " %"+dlNazwy+"s: %"+dlTel+"s\n";
			System.out.printf(format,i,kontakty[i].nazwa,kontakty[i].telefon);	
		}		
		System.out.println("Koniec listy.");
		} else {
			System.out.println("Brak kontaktów w bazie danych.");
		}
	}
	
	private static void dodajKontakt(String nazwa) throws IOException {
		Kontakt nowy = new Kontakt();
		nowy.nazwa = nazwa;
		System.out.print("Podaj nr. telefonu ");
		nowy.telefon = Utils.readLine();
		
		Kontakt[] nowaTabela = new Kontakt[  kontakty.length+1  ];
		for(int i=0; i<kontakty.length; i++){
			nowaTabela[i] = kontakty[i];
		}
		nowaTabela[  nowaTabela.length-1  ] = nowy;
		
		kontakty = nowaTabela;
		
	}
	
	public static void ladowanieDanych() throws IOException, ClassNotFoundException {
		System.out.println("Ładowanie danych...");
		if(plikDanych.exists()){
			kontakty = (Kontakt[]) Utils.czytajObiektZPliku(plikDanych);
		
		System.out.println("Dane załadowano");
		} else {
			System.out.println("Nie ma jeszcze pliku danych. Zostanie utworzony po zakończeniu programu");
			kontakty = new Kontakt[0];
		}
	}
}

