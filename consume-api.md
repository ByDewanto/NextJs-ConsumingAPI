# Consume API Next.js

## Apa itu Konsumsi API?

Konsumsi API (Application Programming Interface) adalah proses menggunakan layanan atau data yang disediakan oleh API eksternal dalam aplikasi kita. Proses ini melibatkan:

- Mengirim permintaan ke endpoint API.
- Memproses respons yang diterima.

Dengan mengonsumsi API, aplikasi dapat berinteraksi dengan layanan pihak ketiga, mengakses data eksternal, serta mengintegrasikan fungsionalitas tambahan tanpa perlu mengembangkannya dari awal.

Pada tutorial ini, kita akan mempelajari cara mengonsumsi API menggunakan Next.js dengan beberapa metode HTTP: GET, POST, PUT, PATCH, dan DELETE. Kita akan menggunakan JSONPlaceholder sebagai API untuk keperluan testing.

## Cara Mengonsumsi API di Next.js dengan Metode HTTP

### 1.GET: Mengambil Data

Metode GET digunakan untuk mengambil data dari API eksternal dan menampilkannya di aplikasi. Dalam contoh ini, kita akan membuat halaman di Next.js yang mengambil data dari JSONPlaceholder dan menampilkan daftar post.

```typescript
// app/posts/page.tsx
"use client";
import { useState, useEffect } from "react";

interface Post {
  id: number;
  title: string;
  body: string;
}

export default function Posts() {
  const [posts, setPosts] = useState<Post[]>([]);

  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/posts"
        );
        const data = await response.json();
        setPosts(data);
      } catch (error) {
        console.error("Error fetching posts:", error);
      }
    };

    fetchPosts();
  }, []);

  return (
    <div className="p-4">
      <h1 className="text-2xl font-bold mb-4">Posts</h1>
      <div className="grid gap-4">
        {posts.map((post) => (
          <div key={post.id} className="border p-4 rounded">
            <h2 className="text-xl font-semibold">{post.title}</h2>
            <p>{post.body}</p>
          </div>
        ))}
      </div>
    </div>
  );
}
```

Kode di atas menggunakan hook `useEffect` untuk menjalankan fungsi `fetchPosts` ketika komponen pertama kali dirender. Fungsi `fetchPosts` ini berfungsi untuk mengambil data dari API eksternal menggunakan metode `fetch`, yang merupakan metode bawaan JavaScript. Setelah data diambil, respons diubah menjadi format JSON dan disimpan dalam state `posts` menggunakan hook `useState`. State ini kemudian digunakan untuk menampilkan data dalam elemen HTML `<ul>`, di mana setiap item `<li>` menampilkan judul `(title)` dan konten `(body)` dari masing-masing post.
