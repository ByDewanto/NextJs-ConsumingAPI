# Consume API Next.js

## Apa itu Konsumsi API?

Konsumsi API (Application Programming Interface) adalah proses menggunakan layanan atau data yang disediakan oleh API eksternal dalam aplikasi kita. Proses ini melibatkan:

- Mengirim permintaan ke endpoint API.
- Memproses respons yang diterima.

Dengan mengonsumsi API, aplikasi dapat berintehttps://github.com/ByDewanto/NextJs-ConsumingAPI.gitraksi dengan layanan pihak ketiga, mengakses data eksternal, serta mengintegrasikan fungsionalitas tambahan tanpa perlu mengembangkannya dari awal.

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

### 2. POST: Membuat Data

Kode berikut adalah komponen `CreatePost` yang dibuat menggunakan `Nextjs`, di mana kita dapat membuat postingan baru dengan mengirimkan data ke endpoint API menggunakan fetch. Komponen ini berfungsi sebagai form input sederhana untuk mengisi judul dan isi postingan.

## Kode Lengkap Komponen

```typescript
"use client";
import { useState } from "react";

export default function CreatePost() {
  const [title, setTitle] = useState("");
  const [body, setBody] = useState("");
  const [message, setMessage] = useState("");
  const [createdPost, setCreatedPost] = useState<any>(null);

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();

    try {
      const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            title,
            body,
            userId: 1,
          }),
        }
      );

      const data = await response.json();
      setMessage("Post created successfully!");
      setTitle("");
      setBody("");
      setCreatedPost(data);
      console.log("Created post:", data);
    } catch (error) {
      setMessage("Error creating post");
      console.error("Error:", error);
    }
  };

  return (
    <div className="p-4 max-w-md mx-auto">
      <h1 className="text-2xl font-bold mb-4">Create New Post</h1>
      {message && (
        <div className="mb-4 p-2 bg-green-100 text-green-700 rounded">
          {message}
        </div>
      )}
      <form onSubmit={handleSubmit} className="space-y-4">
        <div>
          <label className="block mb-1">Title:</label>
          <input
            type="text"
            value={title}
            onChange={(e) => setTitle(e.target.value)}
            className="w-full border p-2 rounded"
            required
          />
        </div>
        <div>
          <label className="block mb-1">Body:</label>
          <textarea
            value={body}
            onChange={(e) => setBody(e.target.value)}
            className="w-full border p-2 rounded"
            rows={4}
            required
          />
        </div>
        <button
          type="submit"
          className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
        >
          Create Post
        </button>
      </form>

      {createdPost && (
        <div className="mt-6 p-4 bg-gray-100 rounded">
          <h2 className="text-xl font-semibold">Created Post:</h2>

          <p>
            <strong>Title:</strong> {createdPost.title}
          </p>
          <p>
            <strong>Body:</strong> {createdPost.body}
          </p>
        </div>
      )}
    </div>
  );
}
```

## Informasi

- Metode POST dalam kode di atas digunakan untuk mengirim data ke server dan membuat entri baru dalam basis data atau layanan API. Di dalam fungsi handleSubmit, POST dikonfigurasi untuk mengirim data JSON berisi title, body, dan userId ke endpoint API `https://jsonplaceholder.typicode.com/posts`.
- URL: Mengarah ke endpoint yang menerima data baru `(https://jsonplaceholder.typicode.com/posts)`.
- Headers: Mengatur Content-Type sebagai application/json untuk menunjukkan bahwa data dikirim dalam format JSON.
- Body: Berisi objek JSON yang mencakup title, body, dan userId, yang dikirim sebagai data baru untuk membuat post.
- Response Handling: Jika pengiriman berhasil, respons JSON diterima dan ditampilkan di halaman. Jika ada kesalahan, pesan error ditampilkan.

## Penjelasan Kode

```typescript
const [title, setTitle] = useState("");
const [body, setBody] = useState("");
const [message, setMessage] = useState("");
const [createdPost, setCreatedPost] = useState<any>(null);
```

- `title` dan `body` digunakan untuk menyimpan input dari user.
- `message` digunakan untuk menampilkan pesan sukses atau error setelah proses pembuatan posting.
- `createdPost` digunakan untuk menyimpan data posting yang berhasil dibuat dan menampilkannya di layar user.

```typescript
const handleSubmit = async (e: React.FormEvent) => {
  e.preventDefault();

  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        title,
        body,
        userId: 1,
      }),
    });

    const data = await response.json();
    setMessage("Post created successfully!");
    setTitle("");
    setBody("");
    setCreatedPost(data);
    console.log("Created post:", data);
  } catch (error) {
    setMessage("Error creating post");
    console.error("Error:", error);
  }
};
```

- Fungsi `handleSubmit` akan dipanggil ketika form di-submit. Fungsi ini akan mengirimkan data posting ke endpoint API menggunakan metode POST.
- Data yang dikirimkan berupa objek JSON yang berisi `title`, `body`, dan `userId`.
- Jika proses berhasil, pesan sukses akan ditampilkan, dan data posting yang berhasil dibuat akan disimpan dalam state `createdPost`.
- Jika terjadi error, pesan error akan ditampilkan di layar.

### 3. PUT: Memperbarui Data

Metode PUT dalam HTTP digunakan untuk memperbarui data yang sudah ada di server. Dalam aplikasi `Next.js`, kita dapat menggunakan metode PUT untuk mengirim perubahan data yang telah diambil dari API dan disesuaikan oleh pengguna. Pada contoh kode ini, kita akan membuat komponen `EditPost` yang berisi form sederhana yang memungkinkan pengguna untuk mengedit postingan yang sudah ada berdasarkan id.

## Kode Lengkap Komponen

```typescript
// app/edit-post/[id]/page.tsx
"use client";
import { useState, useEffect } from "react";
import { useParams } from "next/navigation";

interface Post {
  id: number;
  title: string;
  body: string;
  userId: number;
}

export default function EditPost() {
  const params = useParams();
  const [post, setPost] = useState<Post | null>(null);
  const [title, setTitle] = useState("");
  const [body, setBody] = useState("");
  const [message, setMessage] = useState("");
  const [updatedPost, setUpdatedPost] = useState<Post | null>(null);

  useEffect(() => {
    const fetchPost = async () => {
      try {
        const response = await fetch(
          `https://jsonplaceholder.typicode.com/posts/${params.id}`
        );
        const data = await response.json();
        setPost(data);
        setTitle(data.title);
        setBody(data.body);
      } catch (error) {
        console.error("Error fetching post:", error);
      }
    };

    fetchPost();
  }, [params?.id]);

  const handleUpdate = async (e: React.FormEvent) => {
    e.preventDefault();

    try {
      const response = await fetch(
        `https://jsonplaceholder.typicode.com/posts/${params.id}`,
        {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            id: Number(params.id),
            title,
            body,
            userId: post?.userId,
          }),
        }
      );

      const data = await response.json();
      setMessage("Post updated successfully!");
      setUpdatedPost(data);
      console.log("Updated post:", data);
    } catch (error) {
      setMessage("Error updating pos");
      console.error("Error:", error);
    }
  };

  if (!post) return <div>Loading...</div>;

  return (
    <div className="p-4 max-w-md mx-auto">
      <h1 className="text-2xl font-bold mb-4">Edit Post</h1>
      {message && (
        <div className="mb-4 p-2 bg-green-100 text-green-700 rounded">
          {message}
        </div>
      )}
      <form onSubmit={handleUpdate} className="space-y-4">
        <div>
          <label className="block mb-1">Title:</label>
          <input
            type="text"
            value={title}
            onChange={(e) => setTitle(e.target.value)}
            className="w-full border p-2 rounded text-black"
            required
          />
        </div>
        <div>
          <label className="block mb-1">Body:</label>
          <textarea
            value={body}
            onChange={(e) => setBody(e.target.value)}
            className="w-full border p-2 rounded text-black"
            rows={4}
            required
          />
        </div>
        <button
          type="submit"
          className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
        >
          Update Post
        </button>
      </form>

      {updatedPost && (
        <div className="mt-6 p-4 bg-gray-100 rounded text-black">
          <h2 className="text-xl font-semibold">Updated Post:</h2>
          <p>
            <strong>Title:</strong> {updatedPost.title}
          </p>
          <p>
            <strong>Body:</strong> {updatedPost.body}
          </p>
        </div>
      )}
    </div>
  );
}
```

## Informasi

- Metode PUT digunakan untuk mengirim data yang diperbarui ke server dan memperbarui entri yang sudah ada dalam basis data atau layanan API. Pada komponen ini, fungsi PUT berperan untuk memperbarui postingan yang dipilih berdasarkan ID dari postingan tersebut.
- URL: Mengarah ke endpoint yang menerima pembaruan data `(https://jsonplaceholder.typicode.com/posts/{id})`.
- Headers: Mengatur Content-Type sebagai application/json untuk menunjukkan bahwa data dikirim dalam format JSON.
- Body: Memuat objek JSON dengan id, title, body, dan userId. Data ini dikirim sebagai pembaruan untuk postingan yang sudah ada.
- Response Handling: Jika pengiriman berhasil, respons JSON diterima dan ditampilkan di halaman. Jika ada kesalahan, pesan error ditampilkan.

## Penjelasan Kode

```typescript
const params = useParams();
const [post, setPost] = useState<Post | null>(null);
const [title, setTitle] = useState("");
const [body, setBody] = useState("");
const [message, setMessage] = useState("");
const [updatedPost, setUpdatedPost] = useState<Post | null>(null);
```

- `params`: menggunakan useParams dari Next.js untuk mendapatkan parameter ID dari URL, sehingga kita dapat mengambil dan memperbarui postingan berdasarkan ID tersebut.
- `post`: state untuk menyimpan data postingan yang diambil dari server berdasarkan ID.
- `title` dan `body`: menyimpan input dari pengguna untuk judul dan isi postingan yang ingin diperbarui.
- `message`: menyimpan pesan hasil proses, baik sukses maupun error, setelah pembaruan data dilakukan.
- `updatePost`: menyimpan data postingan yang sudah diperbarui dari respons server, dan digunakan untuk menampilkan hasil pembaruan.

```typescript
useEffect(() => {
  const fetchPost = async () => {
    try {
      const response = await fetch(
        `https://jsonplaceholder.typicode.com/posts/${params.id}`
      );
      const data = await response.json();
      setPost(data);
      setTitle(data.title);
      setBody(data.body);
    } catch (error) {
      console.error("Error fetching post:", error);
    }
  };

  fetchPost();
}, [params?.id]);
```

- `useEffect`: hook ini digunakan untuk mengambil data postingan berdasarkan ID ketika komponen dimuat.
- Fungsi `fetchPost` mengirim permintaan GET ke endpoint API untuk mengambil data postingan.
- Setelah data diterima, data ini diatur ke dalam state `post`, `title`, dan `body`, sehingga form dapat menampilkan data postingan yang siap diperbarui oleh pengguna.
- Jika terjadi error saat mengambil data, error tersebut dicatat di konsol.

```typescript
const handleUpdate = async (e: React.FormEvent) => {
  e.preventDefault();

  try {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/posts/${params.id}`,
      {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          id: Number(params.id),
          title,
          body,
          userId: post?.userId,
        }),
      }
    );

    const data = await response.json();
    setMessage("Post updated successfully!");
    setUpdatedPost(data);
    console.log("Updated post:", data);
  } catch (error) {
    setMessage("Error updating post");
    console.error("Error:", error);
  }
};
```

- `handleUpdate`: fungsi ini dipanggil ketika pengguna mengirim form untuk memperbarui postingan.
- Data yang dikirim melalui metode `PUT` termasuk `id`, `title`, `body`, dan `userId` dari postingan yang ingin diperbarui.
- Setelah pembaruan berhasil, respons dari server disimpan dalam state `updatePost`, dan pesan sukses ditampilkan kepada pengguna. Jika terjadi error, pesan error akan ditampilkan.

```typescript
if (!post) return <div>Loading...</div>;
```

- Kondisi ini digunakan untuk menampilkan pesan `"Loading..."` ketika data postingan belum diambil dari server.

```typescript
<form onSubmit={handleUpdate} className="space-y-4">
  <div>
    <label className="block mb-1">Title:</label>
    <input
      type="text"
      value={title}
      onChange={(e) => setTitle(e.target.value)}
      className="w-full border p-2 rounded text-black"
      required
    />
  </div>
  <div>
    <label className="block mb-1">Body:</label>
    <textarea
      value={body}
      onChange={(e) => setBody(e.target.value)}
      className="w-full border p-2 rounded text-black"
      rows={4}
      required
    />
  </div>
  <button
    type="submit"
    className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
  >
    Update Post
  </button>
</form>
```

- `Form`: memungkinkan pengguna untuk mengubah `title` dan `body` postingan. Saat form di-submit, fungsi `handleUpdate` dipanggil untuk memperbarui data.

```typescript
{
  updatedPost && (
    <div className="mt-6 p-4 bg-gray-100 rounded text-black">
      <h2 className="text-xl font-semibold">Updated Post:</h2>
      <p>
        <strong>Title:</strong> {updatedPost.title}
      </p>
      <p>
        <strong>Body:</strong> {updatedPost.body}
      </p>
    </div>
  );
}
```

- `Render Updated Post`: menampilkan informasi postingan yang sudah diperbarui setelah pembaruan berhasil, menggunakan data yang tersimpan di `updatedPost`.


## 4. Implementasi PATCH Request untuk Update Data

#### Pendahuluan

Dalam pengembangan web, metode PATCH digunakan untuk memperbarui sebagian dari suatu resource. Berbeda dengan PUT yang biasanya menggantikan seluruh data resource, PATCH memungkinkan kita mengirim perubahan hanya pada data tertentu. Misalnya, untuk mengubah `title` atau `body` dari sebuah post, cukup mengirim data yang berubah saja. Ini lebih efisien dan menghemat bandwidth, terutama pada aplikasi yang besar. Pada tutorial ini, kita akan menerapkan PATCH request di Next.js menggunakan React Hooks.

---

### Langkah-langkah Implementasi PATCH Request di Next.js

#### 1. Persiapan Interface dan State Management

Pertama, buat interface dan state untuk komponen kita. Interface digunakan untuk mendefinisikan struktur data post yang akan diambil dan diperbarui.

```typescript
"use client";
import { useState, useEffect } from "react";
import { useParams } from "next/navigation";

interface Post {
  id: number;
  title: string;
  body: string;
  userId: number;
}
```

- **Interface `Post`**: Mendefinisikan struktur data post dengan atribut `id`, `title`, `body`, dan `userId`.
- **State Management**:
  - `post`: Menyimpan data post yang diambil dari API.
  - `title` dan `body`: Menyimpan input pengguna untuk mengedit title dan body post.
  - `message`: Menyimpan pesan notifikasi sukses atau error setelah update.
  - `updatedFields`: Menyimpan data yang berubah untuk ditampilkan setelah update.

```typescript
const [post, setPost] = useState<Post | null>(null);
const [title, setTitle] = useState("");
const [body, setBody] = useState("");
const [message, setMessage] = useState("");
const [updatedFields, setUpdatedFields] = useState<Partial<Post>>({});
```

---

#### 2. Mengambil Data Post Berdasarkan ID

Menggunakan `useEffect`, kita mengambil data post dari API berdasarkan ID dari URL.

```typescript
useEffect(() => {
  const fetchPost = async () => {
    try {
      const response = await fetch(
        `https://jsonplaceholder.typicode.com/posts/${params.id}`
      );
      const data = await response.json();
      setPost(data);
      setTitle(data.title);
      setBody(data.body);
    } catch (error) {
      console.error("Error fetching post:", error);
    }
  };

  fetchPost();
}, [params?.id]);
```

**Penjelasan**:
- `fetchPost` mengambil data post dari API. Hasilnya disimpan di state `post`, `title`, dan `body`, yang akan ditampilkan di form.
- `useEffect` berjalan saat komponen di-mount dan mengambil post berdasarkan `params.id` yang diperoleh dari URL.

---

#### 3. Mengimplementasikan Fungsi PATCH untuk Memperbarui Data

Fungsi ini mengirim permintaan PATCH hanya untuk field yang diubah oleh pengguna.

```typescript
const handlePatchUpdate = async (e: React.FormEvent) => {
  e.preventDefault();

  // Siapkan data yang akan diupdate berdasarkan input pengguna
  const updateData: Partial<Post> = {};
  if (title !== post?.title) updateData.title = title;
  if (body !== post?.body) updateData.body = body;

  if (Object.keys(updateData).length === 0) {
    setMessage("No changes to update");
    return;
  }

  try {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/posts/${params.id}`,
      {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updateData),
      }
    );

    const data = await response.json();
    setMessage("Post updated successfully!");
    setPost(data); // Update local state dengan data baru
    setUpdatedFields(updateData); // Menyimpan field yang diubah saja
    console.log("Patched post:", data);
  } catch (error) {
    setMessage("Error updating post");
    console.error("Error:", error);
  }
};
```

**Penjelasan**:
- **Partial<Post>**: `updateData` hanya berisi field yang diubah.
- **Cek Kondisi**: Mengecek apakah `title` atau `body` berbeda dari data awal. Jika ada perbedaan, field yang berubah ditambahkan ke `updateData`.
- **fetch PATCH request**: Mengirim permintaan PATCH ke server dengan `updateData`. Jika berhasil, `post` dan `updatedFields` diperbarui untuk menampilkan data yang baru.

---

#### 4. Membuat Tampilan Form untuk Edit Data

Form ini memungkinkan pengguna mengedit title dan body dari post. Setelah diubah dan disubmit, pengguna akan melihat notifikasi berdasarkan hasil dari PATCH request.

```typescript
return (
  <div className="p-4 max-w-md mx-auto">
    <h1 className="text-2xl font-bold mb-4">Patch Post</h1>
    {message && (
      <div className="mb-4 p-2 bg-green-100 text-green-700 rounded">
        {message}
      </div>
    )}
    <form onSubmit={handlePatchUpdate} className="space-y-4">
      <div>
        <label className="block mb-1">Title:</label>
        <input
          type="text"
          value={title}
          onChange={(e) => setTitle(e.target.value)}
          className="w-full border p-2 rounded text-black"
          required
        />
      </div>
      <div>
        <label className="block mb-1">Body:</label>
        <textarea
          value={body}
          onChange={(e) => setBody(e.target.value)}
          className="w-full border p-2 rounded text-black"
          rows={4}
          required
        />
      </div>
      <button
        type="submit"
        className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
      >
        Patch Update
      </button>
    </form>

    {Object.keys(updatedFields).length > 0 && (
      <div className="mt-6 p-4 bg-gray-100 rounded text-black">
        <h2 className="text-xl font-semibold">Updated Fields:</h2>
        {updatedFields.title && (
          <p>
            <strong>Title:</strong> {updatedFields.title}
          </p>
        )}
        {updatedFields.body && (
          <p>
            <strong>Body:</strong> {updatedFields.body}
          </p>
        )}
      </div>
    )}
  </div>
);
```

**Penjelasan**:
- **Input dan Textarea**: Mengambil nilai dari `title` dan `body`, yang bisa diedit pengguna.
- **onSubmit**: Saat form disubmit, `handlePatchUpdate` dijalankan untuk memperbarui data.
- **Pesan Notifikasi**: Menampilkan pesan sukses atau error setelah pembaruan.
- **Menampilkan Field yang Diubah**: Hanya field yang diubah (`updatedFields`) yang ditampilkan setelah update, sehingga pengguna bisa melihat perubahan spesifik yang mereka lakukan.

---

### Penjelasan Hasil

Setelah submit:
- **Jika berhasil**: Pesan "Post updated successfully!" ditampilkan, dan data yang baru langsung muncul pada bagian "Updated Fields."
- **Jika error**: Pesan error akan ditampilkan.

### Keuntungan Menggunakan PATCH

- **Efisiensi**: Hanya mengirimkan data yang diubah, membuat permintaan lebih kecil dan cepat.
- **Fleksibilitas**: Dapat memperbarui sebagian data tanpa memengaruhi data lainnya.


## 5. DELETE: Menghapus Data

**Metode DELETE** dalam konteks API Next.js digunakan untuk **menghapus data** dari server. Ini merupakan salah satu dari empat metode HTTP standar (GET, POST, PUT, DELETE) yang digunakan untuk berinteraksi dengan API.

### Cara Kerja Metode DELETE dalam Next.js

1. **Request:**
   * **Client (Frontend):** Ketika pengguna ingin menghapus data, frontend (misalnya, tombol "Hapus") akan mengirimkan permintaan DELETE ke server Next.js. Permintaan ini biasanya berisi ID dari data yang ingin dihapus dalam URL atau body request.
   * **Server (Next.js):** Server Next.js akan menerima permintaan DELETE dan memparsing informasi yang disertakan.

2. **Proses di Server:**
   * **Validasi:** Server akan memvalidasi permintaan untuk memastikan bahwa pengguna memiliki izin untuk menghapus data tersebut dan data yang ingin dihapus memang ada.
   * **Penghapusan Data:** Jika validasi berhasil, server akan menjalankan logika untuk menghapus data dari database atau sumber data lainnya.
   * **Respon:** Server akan mengirimkan respons HTTP dengan status kode 204 (No Content) jika penghapusan berhasil, atau status kode yang sesuai dengan kesalahan yang terjadi (misalnya, 404 Not Found jika data tidak ditemukan).

3. **Client:**
   * **Pembaruan UI:** Frontend akan memperbarui tampilan untuk mencerminkan perubahan data. Misalnya, jika sebuah item dalam daftar dihapus, item tersebut akan dihilangkan dari daftar.

### Contoh Implementasi dalam Next.js

```javascript
// pages/api/posts/[id].js
import { NextApiRequest, NextApiResponse } from 'next';

export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === 'DELETE') {
    const id = req.query.id;

    // Logic untuk menghapus data dengan ID tertentu
    // Misalnya, menggunakan database MongoDB
    try {
      await db.collection('posts').deleteOne({ _id: ObjectId(id) });
      res.status(204).end();
    } catch (error) {
      console.error(error);
      res.status(500).json({ message: 'Internal Server Error' });
    }
  } else {
    res.status(405).end(); // Method Not Allowed
  }
}
```

### Penjelasan Kode:

* **API Route:** File `pages/api/posts/[id].js` mendefinisikan sebuah API route untuk menangani permintaan terkait postingan dengan ID tertentu.
* **Metode DELETE:** Kode hanya akan dijalankan jika metode permintaan adalah DELETE.
* **Pengambilan ID:** ID postingan yang akan dihapus diambil dari query parameter `id`.
* **Logika Penghapusan:** Bagian ini akan diganti dengan logika aktual untuk menghapus data dari database Anda. Contoh di atas menggunakan MongoDB.
* **Respon:** Jika penghapusan berhasil, status kode 204 akan dikirimkan. Jika terjadi kesalahan, status kode 500 akan dikirimkan beserta pesan error.

### Pertimbangan Tambahan:

* **Otorisasi:** Pastikan Anda menerapkan mekanisme otorisasi untuk memastikan hanya pengguna yang berwenang yang dapat menghapus data.
* **Validasi:** Lakukan validasi terhadap data yang diterima untuk mencegah serangan injeksi dan kesalahan lainnya.
* **Transaksi:** Jika penghapusan melibatkan beberapa operasi, gunakan transaksi untuk menjaga konsistensi data.
* **Logging:** Catat semua permintaan DELETE untuk tujuan debugging dan analisis.

### Penggunaan dalam Frontend

```javascript
import axios from 'axios';

const deletePost = async (id) => {
  try {
    await axios.delete(`/api/posts/${id}`);
    // Perbarui UI setelah penghapusan berhasil
  } catch (error) {
    console.error(error);
  }
};
```

Metode DELETE dalam API Next.js adalah sebuah mekanisme untuk menghapus data dari server. Ketika seorang pengguna ingin menghapus data tertentu (misalnya, sebuah postingan blog), frontend akan mengirimkan permintaan DELETE ke server Next.js. Server kemudian akan memvalidasi permintaan, menghapus data yang sesuai dari database, dan mengirimkan respons kepada frontend.