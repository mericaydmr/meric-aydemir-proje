import java.util.*;

class SinemaUygulamasi {

    static class Film {
        String id;
        String ad;
        int sure;
        boolean[][] koltuklar = new boolean[5][5]; // 5x5 koltuk

        public Film(String id, String ad, int sure) {
            this.id = id;
            this.ad = ad;
            this.sure = sure;
        }

        @Override
        public String toString() {
            return "🎬 " + id + " - " + ad + " (" + sure + " dk)";
        }

        public void koltuklariGoster() {
            System.out.println("\n🎟️ Koltuklar (X=dolu, O=boş):");
            for (int i = 0; i < 5; i++) {
                for (int j = 0; j < 5; j++) {
                    System.out.print(koltuklar[i][j] ? " X " : " O ");
                }
                System.out.println(" <- Satır " + (i + 1));
            }
            System.out.println(" 1  2  3  4  5 (Sütun)");
        }

        public boolean koltukMusaitMi(int satir, int sutun) {
            return !koltuklar[satir][sutun];
        }

        public void koltukRezerveEt(int satir, int sutun) {
            koltuklar[satir][sutun] = true;
        }
    }

    static class Musteri {
        String id;
        String ad;
        String soyad;

        public Musteri(String id, String ad, String soyad) {
            this.id = id;
            this.ad = ad;
            this.soyad = soyad;
        }

        @Override
        public String toString() {
            return "👤 " + id + " - " + ad + " " + soyad;
        }
    }

    static class Bilet {
        String id;
        Musteri musteri;
        Film film;
        int satir;
        int sutun;

        public Bilet(String id, Musteri musteri, Film film, int satir, int sutun) {
            this.id = id;
            this.musteri = musteri;
            this.film = film;
            this.satir = satir;
            this.sutun = sutun;
        }

        @Override
        public String toString() {
            return "🎟️ " + id + " - " + musteri.ad + " " + musteri.soyad +
                    " --> " + film.ad + " (Koltuk: " + (satir + 1) + "." + (sutun + 1) + ")";
        }
    }

    static ArrayList<Film> filmler = new ArrayList<>();
    static ArrayList<Musteri> musteriler = new ArrayList<>();
    static ArrayList<Bilet> biletler = new ArrayList<>();
    static int filmSayac = 1;
    static int musteriSayac = 1;
    static int biletSayac = 1;

    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int secim;

        do {
            System.out.println("\n🎞️ === SİNEMA SİSTEMİ ===");
            System.out.println("1. Film Ekle");
            System.out.println("2. Film Listele");
            System.out.println("3. Müşteri Ekle");
            System.out.println("4. Müşteri Listele");
            System.out.println("5. Bilet Oluştur (Koltuklu)");
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
                case 0 -> System.out.println("👋 Programdan çıkılıyor...");
                default -> System.out.println("❗ Geçersiz seçim.");
            }
        } while (secim != 0);
    }

    static void filmEkle() {
        System.out.print("Film adı: ");
        String ad = scanner.nextLine();
        System.out.print("Film süresi (dk): ");
        int sure = Integer.parseInt(scanner.nextLine());

        String id = "F" + filmSayac++;
        filmler.add(new Film(id, ad, sure));
        System.out.println("✅ Film eklendi: " + ad);
    }

    static void filmListele() {
        if (filmler.isEmpty()) {
            System.out.println("⚠️ Hiç film yok.");
            return;
        }
        for (Film f : filmler) {
            System.out.println(f);
        }
    }

    static void musteriEkle() {
        System.out.print("Müşteri adı: ");
        String ad = scanner.nextLine();
        System.out.print("Müşteri soyadı: ");
        String soyad = scanner.nextLine();

        String id = "M" + musteriSayac++;
        musteriler.add(new Musteri(id, ad, soyad));
        System.out.println("✅ Müşteri eklendi.");
    }

    static void musteriListele() {
        if (musteriler.isEmpty()) {
            System.out.println("⚠️ Hiç müşteri yok.");
            return;
        }
        for (Musteri m : musteriler) {
            System.out.println(m);
        }
    }

    static void biletOlustur() {
        if (filmler.isEmpty() || musteriler.isEmpty()) {
            System.out.println("⚠️ Önce müşteri ve film eklemelisiniz.");
            return;
        }

        System.out.print("Müşteri ID: ");
        String mid = scanner.nextLine();
        Musteri musteri = musteriler.stream()
                .filter(m -> m.id.equalsIgnoreCase(mid))
                .findFirst().orElse(null);

        System.out.print("Film ID: ");
        String fid = scanner.nextLine();
        Film film = filmler.stream()
                .filter(f -> f.id.equalsIgnoreCase(fid))
                .findFirst().orElse(null);

        if (musteri == null || film == null) {
            System.out.println("❌ Müşteri veya film bulunamadı.");
            return;
        }

        film.koltuklariGoster();

        int satir, sutun;
        while (true) {
            System.out.print("Satır (1-5): ");
            satir = Integer.parseInt(scanner.nextLine()) - 1;
            System.out.print("Sütun (1-5): ");
            sutun = Integer.parseInt(scanner.nextLine()) - 1;

            if (satir < 0 || satir >= 5 || sutun < 0 || sutun >= 5) {
                System.out.println("❗ Geçersiz koltuk.");
                continue;
            }

            if (film.koltukMusaitMi(satir, sutun)) {
                film.koltukRezerveEt(satir, sutun);
                break;
            } else {
                System.out.println("❌ Bu koltuk dolu, başka seçin.");
            }
        }

        String bid = "B" + biletSayac++;
        biletler.add(new Bilet(bid, musteri, film, satir, sutun));
        System.out.println("✅ Bilet başarıyla oluşturuldu.");
    }

    static void biletListele() {
        if (biletler.isEmpty()) {
            System.out.println("⚠️ Henüz hiç bilet yok.");
            return;
        }
        for (Bilet b : biletler) {
            System.out.println(b);
        }
    }
}

