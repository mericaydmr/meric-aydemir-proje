import java.util.*;

class SinemaSistemi {


    static class Film {
        String ad;
        int sure;
        String tur;

        public Film(String ad, int sure, String tur) {
            this.ad = ad;
            this.sure = sure;
            this.tur = tur;
        }

        @Override
        public String toString() {
            return "🎬 " + ad + " (" + sure + " dk) - Tür: " + tur;
        }
    }


    static class Musteri {
        String ad;
        String email;

        public Musteri(String ad, String email) {
            this.ad = ad;
            this.email = email;
        }

        @Override
        public String toString() {
            return "👤 " + ad + " - " + email;
        }
    }


    static class Bilet {
        Musteri musteri;
        Film film;

        public Bilet(Musteri musteri, Film film) {
            this.musteri = musteri;
            this.film = film;
        }

        @Override
        public String toString() {
            return " " + musteri.ad + " - " + film.ad + " (" + film.sure + " dk)";
        }
    }


    static Film[] filmler = new Film[10]; // Maksimum 10 film
    static Musteri[] musteriler = new Musteri[20]; // Maksimum 20 müşteri
    static Bilet[] biletler = new Bilet[20]; // Maksimum 20 bilet
    static int filmSayac = 0;
    static int musteriSayac = 0;
    static int biletSayac = 0;

    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int secim;

        do {
            System.out.println("\n === SİNEMA SİSTEMİ ===");
            System.out.println("1. Film Ekle");
            System.out.println("2. Film Listele");
            System.out.println("3. Müşteri Ekle");
            System.out.println("4. Müşteri Listele");
            System.out.println("5. Bilet Oluştur");
            System.out.println("6. Bilet Listele");
            System.out.println("0. Çıkış");
            System.out.print("Seçiminiz: ");
            secim = Integer.parseInt(scanner.nextLine());

            switch (secim) {
                case 1 -> filmEkle();
                case 2 -> filmListele();
                case 3 -> musteriEkle();
                case 4 -> musteriListele();
                case 5 -> biletOlustur();
                case 6 -> biletListele();
                case 0 -> System.out.println(" Programdan çıkılıyor...");
                default -> System.out.println(" Geçersiz seçim.");
            }
        } while (secim != 0);
    }

    static void filmEkle() {
        if (filmSayac >= 10) {
            System.out.println(" Maksimum film sayısına ulaşıldı!");
            return;
        }

        System.out.print("Film adı: ");
        String ad = scanner.nextLine();
        System.out.print("Film süresi (dk): ");
        int sure = Integer.parseInt(scanner.nextLine());
        System.out.print("Film türü: ");
        String tur = scanner.nextLine();

        filmler[filmSayac++] = new Film(ad, sure, tur);
        System.out.println(" Film eklendi: " + ad);
    }

    static void filmListele() {
        if (filmSayac == 0) {
            System.out.println(" Hiç film yok.");
            return;
        }
        System.out.println(" Film Listesi:");
        for (int i = 0; i < filmSayac; i++) {
            System.out.println(filmler[i]);
        }
    }

    static void musteriEkle() {
        if (musteriSayac >= 20) {
            System.out.println("Maksimum müşteri sayısına ulaşıldı!");
            return;
        }

        System.out.print("Müşteri adı: ");
        String ad = scanner.nextLine();
        System.out.print("Müşteri email: ");
        String email = scanner.nextLine();

        musteriler[musteriSayac++] = new Musteri(ad, email);
        System.out.println(" Müşteri eklendi: " + ad);
    }

    static void musteriListele() {
        if (musteriSayac == 0) {
            System.out.println(" Hiç müşteri yok.");
            return;
        }
        System.out.println(" Müşteri Listesi:");
        for (int i = 0; i < musteriSayac; i++) {
            System.out.println(musteriler[i]);
        }
    }

    static void biletOlustur() {
        if (filmSayac == 0 || musteriSayac == 0) {
            System.out.println(" Önce film ve müşteri eklemelisiniz.");
            return;
        }

        System.out.print("Müşteri ID: ");
        String mid = scanner.nextLine();
        Musteri musteri = null;
        for (int i = 0; i < musteriSayac; i++) {
            if (musteriler[i].email.equals(mid)) {
                musteri = musteriler[i];
                break;
            }
        }

        if (musteri == null) {
            System.out.println(" Müşteri bulunamadı.");
            return;
        }

        System.out.print("Film ID: ");
        String fid = scanner.nextLine();
        Film film = null;
        for (int i = 0; i < filmSayac; i++) {
            if (filmler[i].ad.equals(fid)) {
                film = filmler[i];
                break;
            }
        }

        if (film == null) {
            System.out.println(" Film bulunamadı.");
            return;
        }

        if (biletSayac >= 20) {
            System.out.println(" Maksimum bilet sayısına ulaşıldı!");
            return;
        }

        biletler[biletSayac++] = new Bilet(musteri, film);
        System.out.println(" Bilet başarıyla oluşturuldu.");
    }

    static void biletListele() {
        if (biletSayac == 0) {
            System.out.println(" Hiç bilet yok.");
            return;
        }
        System.out.println(" Bilet Listesi:");
        for (int i = 0; i < biletSayac; i++) {
            System.out.println(biletler[i]);
        }
    }
}

