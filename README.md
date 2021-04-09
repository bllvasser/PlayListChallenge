# PlayListChallenge
package com.company;

import java.util.ArrayList;

public class Song {
    private String title;
    private double duration;


    public Song(String title, double duration) {
        this.title = title;
        this.duration = duration;

    }

    public Song() {

    }

    public String getTitle() {
        return title;
    }


    @Override
    public String toString() {
        return this.title + " : " + this.duration;
    }
}
package com.company;

import java.util.ArrayList;
import java.util.LinkedList;

public class Album extends Song {
    private ArrayList<Song> songs;
    private String name;
    private String artist;


    public Album(String name) {
        this.songs = new ArrayList<Song>();
        this.name = name;
        this.artist = artist;
    }

    public boolean addSong(String title, double duration) {

        if (findSong(title) == null) {
            this.songs.add(new Song(title, duration));
            return true;
        }
        return false;
    }

    private Song findSong(String title) {

        for (Song checkedSong : this.songs) {
            if (checkedSong.getTitle().equals(title)) {
                return checkedSong;
            }
        }
        return null;
    }

    public boolean addToPlayList(int trackNumber, LinkedList<Song> playList) {
        int index = trackNumber - 1;
        if ((index >= 0) && (index <= this.songs.size())) {
            playList.add(this.songs.get(index));
            return true;
        }
        System.out.println(" This album does not have a track " + trackNumber);
        return false;
    }

    public boolean addToPlayList(String title, LinkedList<Song> playList) {
        Song checkedSong = findSong(title);
        if (checkedSong != null) {
            playList.add(checkedSong);
            return true;
        }
        System.out.println(" The song " + title + " is not in the album ");
        return false;
    }

}
package com.company;

import java.util.*;

public class Main {

    private static ArrayList<Album> albums = new ArrayList<Album>();


    public static void main(String[] args) {


        Album album = new Album("Linkin Park");
        album.addSong("Crawling", 4.2);
        album.addSong("From the inside", 3.20);
        album.addSong("Papercut", 4.5);
        album.addSong("Given Up", 4.2);
        album.addSong("Running Away", 3.20);
        album.addSong("Bleed Out", 4.5);
        albums.add(album);

        Album album2 = new Album("DMX");
        album2.addSong("Damien", 4.5);
        album2.addSong("Its Dark And Hell Is Hot", 6.1);
        album2.addSong("What These Chickens Want", 4.0);
        album2.addSong("Omen", 4.5);
        album2.addSong("Let Me In", 6.1);
        album2.addSong("Lord Forgive us", 4.0);
        albums.add(album2);

        LinkedList<Song> playList = new LinkedList<Song>();
        albums.get(0).addToPlayList("LinkinPark", playList);
        albums.get(1).addToPlayList(1, playList);
        albums.get(0).addToPlayList(2, playList);
        albums.get(1).addToPlayList(3, playList);
        albums.get(0).addToPlayList(4, playList);
        albums.get(1).addToPlayList(5, playList);
        albums.get(0).addToPlayList(6, playList);
        play(playList);
    }

    private static void printMenu() {
        System.out.println("\nAvailable menu: \n press");
        System.out.println("0 - to quit playlist\n" +
                " 1 - to go forward \n" +
                " 2 - to go backward \n" +
                " 3 - to replay song \n" +
                " 4 - to print playlist \n" +
                " 5 - to print menu\n"
                +
                " 6 - to remove song\n");
    }

    private static void printList(LinkedList<Song> playList) {
        Iterator<Song> iterator = playList.iterator();
        System.out.println("=================");
        while (iterator.hasNext()) {
            System.out.println(iterator.next().toString());
        }
        System.out.println("==============");
    }

    private static void play(LinkedList<Song> playList) {

        Scanner scanner = new Scanner(System.in);
        boolean quit = false;
        boolean forward = true;
        ListIterator<Song> listIterator = playList.listIterator();
        if (playList.size() == 0) {
            System.out.println("No songs in playlist");
            return;
        } else {
            System.out.println(" Now playing " + listIterator.next().toString());
            printMenu();
        }

        while (!quit) {
            int action = scanner.nextInt();
            scanner.nextLine();

            switch (action) {
                case 0:
                    System.out.println("Play list complete");
                    quit = true;
                    break;

                case 1:
                    if (!forward) {
                        if (listIterator.hasNext()) {
                            listIterator.next();
                        }
                        forward = true;
                    }
                    if (listIterator.hasNext()) {
                        System.out.println("Now playing " + listIterator.next().toString());
                    } else {
                        System.out.println("We have reached the end of playlist");
                        forward = false;
                    }
                    break;

                case 2:
                    if (false) {
                        if (listIterator.hasPrevious()) {
                            listIterator.previous();
                        }
                        forward = false;
                    }
                    if (listIterator.hasPrevious()) {
                        System.out.println("Now playing " + listIterator.previous().toString());
                    } else {
                        System.out.println("We are at the start of the playlist");
                        forward = true;
                    }
                    break;

                case 3:
                    if (forward) {
                        if (listIterator.hasPrevious()) {
                            System.out.println("Now replaying " + listIterator.previous().toString());
                            forward = false;
                        } else {
                            System.out.println("We are at the start of the list");
                        }
                    } else if (listIterator.hasNext()) {
                        System.out.println("Now replaying " + listIterator.next().toString());
                        forward = true;
                    } else {
                        System.out.println("We have reached the end of the list");
                    }

                    break;

                case 4:
                    printList(playList);
                    break;

                case 5:
                    printMenu();
                    break;

                case 6:
                    if (playList.size() > 0) {
                        listIterator.remove();
                        if (listIterator.hasNext()) {
                            System.out.println("Now playing " + listIterator.next());
                        } else if (listIterator.hasPrevious()) {
                            System.out.println("Now playing " + listIterator.previous());
                        }

                    }
                    break;
            }
        }
    }

}
/Library/Java/JavaVirtualMachines/amazon-corretto-11.jdk/Contents/Home/bin/java "-javaagent:/Applications/IntelliJ IDEA CE.app/Contents/lib/idea_rt.jar=63269:/Applications/IntelliJ IDEA CE.app/Contents/bin" -Dfile.encoding=UTF-8 -classpath /Users/Bilaal/IdeaProjects/PlayListChallenge/out/production/PlayListChallenge com.company.Main
 The song LinkinPark is not in the album 
 Now playing Damien : 4.5

Available menu: 
 press
0 - to quit playlist
 1 - to go forward 
 2 - to go backward 
 3 - to replay song 
 4 - to print playlist 
 5 - to print menu
 6 - to remove song

