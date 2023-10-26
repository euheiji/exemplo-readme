# ProjetoVanEscolar
[ ![NPM](https://img.shields.io/npm/l/reacts)](https://https://github.com/euheiji/exemplo-readme/blob/main/LICENSE)

## Sobre o projeto

http://localhost:8080/swagger-ui.html

O sistema permite que o pais ou responsável pelo aluno possuam um controle  de informações sobre o motorista,  onde podendo buscar por novos motoristas, ter uma localização atual sobre onde o aluno está na van.

# Documentacao

## Motorista

```java
public class MotoristaService {

    @Autowired
    private MotoristaRepository motoristaRepository;

    @Autowired
    private ResponsavelRepository responsavelRepository;

    @Autowired
    private ModelMapper modelMapper;

    public MotoristaAutomovelDto cadastrarMotorista(MotoristaAutomovelDto motoristaAutomovelDto) {

        Motorista motorista = modelMapper.map(motoristaAutomovelDto, Motorista.class);
        Automovel automovel = modelMapper.map(motoristaAutomovelDto.getAutomovel(), Automovel.class);

        motorista.setAutomovel(automovel);
        motoristaRepository.save(motorista);


        return modelMapper.map(motorista, MotoristaAutomovelDto.class);
    }

    public Page<MotoristaDto> acharMotorista(String cidade, String bairro, Pageable pageable) {

        return motoristaRepository.findByEndereco_CidadeOrEndereco_Bairro(cidade, bairro, pageable)
                .map(motorista -> modelMapper.map(motorista, MotoristaDto.class));
    }


    public Page<ResponsavelDto> verPedidosCorridas(Long idMotorista, Pageable pageable) {

        return responsavelRepository.acharPorPedidoFeito(idMotorista, pageable)
                .map(responsavel -> modelMapper.map(responsavel, ResponsavelDto.class));
    }


    public ResponsavelMotoristaDto aceitarCorrida(Long idMotorista, PedidoCorridaMotoristaDto pedidoCorridaMotoristaDto) {

        Responsavel responsavel = responsavelRepository.findById(pedidoCorridaMotoristaDto.getIdResponsavel())
                .orElseThrow(UsuarioNaoEncontrado::new);

        Motorista motorista = motoristaRepository.findById(idMotorista)
                .orElseThrow(UsuarioNaoEncontrado::new);


        responsavel.setStatusPedidoCorrida(StatusPedidoCorrida.Pedido_Aceito);
        motorista.setStatusPedidoCorrida(StatusPedidoCorrida.Pedido_Aceito);


        responsavel.setMotorista(motorista);

        responsavelRepository.save(responsavel);

        return modelMapper.map(responsavel, ResponsavelMotoristaDto.class);
    }

    @Transactional
    public ResponsavelMotoristaDto negarCorrida(Long idMotorista, PedidoCorridaMotoristaDto pedidoCorridaMotoristaDto) {

        Responsavel responsavel = responsavelRepository.findById(pedidoCorridaMotoristaDto.getIdResponsavel())
                .orElseThrow(UsuarioNaoEncontrado::new);

        Motorista motorista = motoristaRepository.findById(idMotorista)
                .orElseThrow(UsuarioNaoEncontrado::new);

        responsavel.setStatusPedidoCorrida(StatusPedidoCorrida.Pedido_Negado);
        motorista.setStatusPedidoCorrida(StatusPedidoCorrida.Pedido_Negado);

        responsavel.setMotorista(motorista);

        responsavelRepository.save(responsavel);


        return modelMapper.map(responsavel, ResponsavelMotoristaDto.class);
    }

    public AtualizaMotoristaDto atualizarMotorista(Long idMotorista, AtualizaMotoristaDto dto) {


        motoristaRepository.findById(idMotorista).orElseThrow(UsuarioNaoEncontrado::new);
        Motorista motorista = modelMapper.map(dto, Motorista.class);

        Automovel automovel = modelMapper.map(dto.getAutomovel(), Automovel.class);

        motorista.setId(idMotorista);
        motorista.setNome(dto.getNome());
        motorista.setCnh(dto.getCnh());
        motorista.setTelefone(dto.getTelefone());
        motorista.setEndereco(dto.getEndereco());

        motorista.setAutomovel(automovel);

        motoristaRepository.save(motorista);

        return modelMapper.map(motorista, AtualizaMotoristaDto.class);
    }


    public MotoristaAutomovelDto findById(Long idMotorista) {

        Motorista motorista = motoristaRepository.findById(idMotorista).orElseThrow(UsuarioNaoEncontrado::new);
        return modelMapper.map(motorista, MotoristaAutomovelDto.class);

    }
}
```
## Responsável

```Java
public class ResponsavelService {


    @Autowired
    private ResponsavelRepository responsavelRepository;

    @Autowired
    private MotoristaRepository motoristaRepository;
    @Autowired
    private ModelMapper modelMapper;

    public ResponsavelDto cadastrarResponsavel(ResponsavelDto dto) {

        Responsavel responsavel = modelMapper.map(dto, Responsavel.class);


        responsavel.getAluno().forEach(aluno -> aluno.setResponsavel(responsavel));

        responsavelRepository.save(responsavel);

        return modelMapper.map(responsavel, ResponsavelDto.class);
    }

    public ResponsavelMotoristaDto solicitarCorrida(Long idResponsavel, PedidoCorridaResponsavelDto pedidoCorrida) {

        Responsavel responsavel = responsavelRepository.findById(idResponsavel).orElseThrow(UsuarioNaoEncontrado::new);
        Motorista motorista = motoristaRepository.findById(pedidoCorrida.getIdMotorista()).orElseThrow(UsuarioNaoEncontrado::new);

        responsavel.setStatusPedidoCorrida(StatusPedidoCorrida.Feito_Pedido);
        motorista.setStatusPedidoCorrida(StatusPedidoCorrida.Feito_Pedido);
        responsavel.setMotorista(motorista);

        responsavelRepository.save(responsavel);

        return modelMapper.map(responsavel, ResponsavelMotoristaDto.class);

    }

    public ResponsavelMotoristaDto findByid(Long id) {

        Responsavel responsavel = responsavelRepository.findById(id).orElseThrow(UsuarioNaoEncontrado::new);

        return modelMapper.map(responsavel, ResponsavelMotoristaDto.class);

    }
}
```







# Layout web

# Back end

# Front end

## Back end
Pré-requisito: Java 18

## Nome dos participantes

Ana Paula,
Heiji,
Ezio,
Quezia.








