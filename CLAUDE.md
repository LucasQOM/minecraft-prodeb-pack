# Prodeb Pack — packwiz modpack

Modpack Minecraft 1.21.1 NeoForge 21.1.209, gerenciado via packwiz.

## Caminhos importantes

- **Pack (este repo):** `D:\Projetos Programação\prodeb-pack\`
- **packwiz.exe:** `D:\packwiz.exe`
- **Instância cliente (CurseForge):** `C:\Users\lucas\curseforge\minecraft\Instances\Prodeb\`
- **Servidor:** `F:\crafty-controller\servers\c7765537-b400-4f06-b6d0-dd2a6008d942\`

## Estrutura do repo

```
pack.toml          — metadados do pack (nome, versão, MC, NeoForge)
index.toml         — índice gerado automaticamente pelo packwiz (não editar)
mods/              — um .pw.toml por mod (sem JARs, só metadados)
config/            — arquivos de config incluídos no pack (opcional)
```

## Workflow para atualizar o modpack

### Adicionar um mod novo

```bash
cd "D:\Projetos Programação\prodeb-pack"

# Busca e instala pelo Modrinth
D:\packwiz.exe modrinth install <slug-ou-url>

# Busca e instala pelo CurseForge
D:\packwiz.exe curseforge install <slug-ou-url>
```

Depois de adicionar, verificar se o mod é client-only e ajustar `side` se necessário (ver seção abaixo).

### Atualizar mods existentes

```bash
# Atualizar um mod específico
D:\packwiz.exe update <nome-do-mod>

# Atualizar todos os mods
D:\packwiz.exe update --all
```

### Publicar atualização (clientes e servidor recebem automaticamente)

```bash
git add .
git commit -m "descrição da mudança"
git push
```

Após o push, o GitHub Pages atualiza em ~1 min. Na próxima vez que um amigo abrir o launcher (ou o servidor reiniciar), os mods são sincronizados automaticamente.

### Testar localmente antes de publicar

```bash
D:\packwiz.exe serve
# Inicia servidor local em http://localhost:8080/pack.toml
# Configure o launcher para apontar para essa URL durante testes
```

### Regenerar index manualmente (se editar arquivos à mão)

```bash
D:\packwiz.exe refresh
```

## Configuração de `side` nos mods

Cada mod em `mods/*.pw.toml` pode ter:
- `side = "both"` — instala no cliente e no servidor (padrão)
- `side = "client"` — só no cliente (HUD, visuais, shaders, minimapa)
- `side = "server"` — só no servidor

Mods client-only neste pack (adicionar `side = "client"` se ainda não tiver):
- ShoulderSurfing, XaerosWorldMap, freecam, notenoughanimations
- cosmeticarmorreworked, camera, MouseTweaks, DistantHorizons
- NaturesCompass, configured, inventorysorter

## URL do pack (após habilitar GitHub Pages)

```
https://<usuario>.github.io/prodeb-pack/pack.toml
```

Substituir `<usuario>` pelo nome de usuário do GitHub.

## Configuração dos clientes

### AtLauncher e Prism/MultiMC
Pre-launch command na instância:
```
java -jar packwiz-installer-bootstrap.jar https://<usuario>.github.io/prodeb-pack/pack.toml
```

O `packwiz-installer-bootstrap.jar` fica na pasta `.minecraft` da instância.

### CurseForge App (sem auto-update)
Exportar manualmente e redistribuir:
```bash
D:\packwiz.exe curseforge export
```

## Configuração do servidor

O `run.bat` do servidor deve rodar o installer antes de iniciar:
```batch
java -jar packwiz-installer-bootstrap.jar -s server https://<usuario>.github.io/prodeb-pack/pack.toml
java @user_jvm_args.txt @libraries/net/neoforged/neoforge/21.1.209/win_args.txt nogui %*
```
